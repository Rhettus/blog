---
layout: post
title: "ZFS Disk performance troubleshooting under FreeBSD"
date: 2021-06-20 19:32:20 +0300
description: # Add post description (optional)
img: # Add image post (optional)
categories: Tech
---

I have had some issues with my RAIDZ2 storage pool and general performance under XigmaNAS/FreeBSD. I was seeing issues with my VirtualBox VM which runs BlueIris security sotfware under a Windows 10 VM being generally laggy and unresponsive. I tried all the usual trick which write caching etc but it was still slow.

I thought I would try to track down my issues and the porcess I go through and document it here. 

During a standard resilver I found the following performance.

```
ss-nas-01: ~# zpool status
  pool: storage
 state: DEGRADED
status: One or more devices is currently being resilvered.  The pool will
        continue to function, possibly in a degraded state.
action: Wait for the resilver to complete.
  scan: resilver in progress since Sun Jun 20 15:24:20 2021
        1.35T scanned at 98.9M/s, 1.31T issued at 96.3M/s, 20.8T total
        109G resilvered, 6.33% done, 2 days 10:54:20 to go
config:

        NAME                        STATE     READ WRITE CKSUM
        storage                     DEGRADED     0     0     0
          raidz2-0                  DEGRADED     0     0     0
            gpt/raidz2disk1         ONLINE       0     0     0
            gpt/raidz2disk2         ONLINE       0     0     0
            replacing-2             OFFLINE      0     0     0
              16730900509885275321  OFFLINE      0     0     0  was /dev/gpt/raidz2disk3
              gpt/zfs-V9GP073M      ONLINE       0     0     0
            gpt/raidz2disk4         ONLINE       0     0     0
            gpt/raidz2disk5         ONLINE       0     0     0
            gpt/raidz2disk6         ONLINE       0     0     0
            gpt/raidz2disk7         ONLINE       0     0     0
            gpt/raidz2disk0         ONLINE       0     0     0
```

## Use "dd" for basic read tests

Warning ensure you do not write to the disks or it will corrupt disk disk int he ZFS pool. Read only please note the input file (if) is the disk and the output file (of) is `/dev/null`!

```time dd if=/dev/ada6 of=/dev/null bs=1M count=10000```

## Find specific disk performance information with "diskinfo"

I iterated through all my disks with the following command

`diskinfo -t /dev/adaX`

Sample output is

```
/dev/adaX
        512             # sectorsize
        4000787030016   # mediasize in bytes (3.6T)
        7814037168      # mediasize in sectors
        4096            # stripesize
        0               # stripeoffset
        7752021         # Cylinders according to firmware.
        16              # Heads according to firmware.
        63              # Sectors according to firmware.
        WDC WD40EFRX-68N32N0    # Disk descr.
        WD-WCC7K4KPPRL7 # Disk ident.
        id1,enc@n3061686369656d30/type@0/slot@8/elmdesc@Slot_07 # Physical path
        No              # TRIM/UNMAP support
        5400            # Rotation rate in RPM
        Not_Zoned       # Zone Mode

Seek times:
        Full stroke:      250 iter in  17.351104 sec =   69.404 msec
        Half stroke:      250 iter in  13.592548 sec =   54.370 msec
        Quarter stroke:   500 iter in  24.683677 sec =   49.367 msec
        Short forward:    400 iter in  10.075415 sec =   25.189 msec
        Short backward:   400 iter in   4.727082 sec =   11.818 msec
        Seq outer:       2048 iter in   0.866425 sec =    0.423 msec
        Seq inner:       2048 iter in   0.897876 sec =    0.438 msec

Transfer rates:
        outside:       102400 kbytes in   1.903863 sec =    53785 kbytes/sec
        middle:        102400 kbytes in   1.811469 sec =    56529 kbytes/sec
        inside:        102400 kbytes in   2.676795 sec =    38255 kbytes/sec
```
## Use "gstat" to find which disk is busy

This is a great uility which really helped me hone is on which disk was holding up the resilver. I have been looking at the %busy column and found that my ada0 device was constantly at or over 100%

`gstat -I 10s`


```
dT: 0.057s  w: 10.000s
 L(q)  ops/s    r/s   kBps   ms/r    w/s   kBps   ms/w   %busy Name
    0      0      0      0    0.0      0      0    0.0    0.0| md0
    0      0      0      0    0.0      0      0    0.0    0.0| md1
   10    316    316  36079   18.6      0      0    0.0  111.8| ada0
    1    246      0      0    0.0    246   1474    0.9   22.6| ada1
    8    316    316  34745   17.1      0      0    0.0   95.0| ada2
    5    491    491  55522   13.5      0      0    0.0  112.7| ada3
   16    667    667  80511   11.4      0      0    0.0   99.1| ada4
    0      0      0      0    0.0      0      0    0.0    0.0| ada5
    9    193    193  20356   25.2      0      0    0.0  101.2| ada6
    3    614    614  73772   13.7      0      0    0.0  112.5| ada7
    0      0      0      0    0.0      0      0    0.0    0.0| da0
    2     70     70  36079   29.2      0      0    0.0  111.8| ada0p1
    2     70     70  36079   29.2      0      0    0.0  111.8| gpt/raidz2disk2
    2     70     70  32499   20.4      0      0    0.0   93.0| ada2p1
    2    105    105  39799   16.2      0      0    0.0   92.8| ada3p1
    2    140    140  80511   13.9      0      0    0.0   99.1| ada4p1
    0      0      0      0    0.0      0      0    0.0    0.0| ada5p1
    0      0      0      0    0.0      0      0    0.0    0.0| gpt/gptroot
    2     53     53  18110   25.4      0      0    0.0   99.7| ada6p1
    2    105    105  58049   15.9      0      0    0.0  100.9| ada7p1
    0      0      0      0    0.0      0      0    0.0    0.0| ufsid/60985e4400793734
    0      0      0      0    0.0      0      0    0.0    0.0| da0p1
    0      0      0      0    0.0      0      0    0.0    0.0| da0p2
    0      0      0      0    0.0      0      0    0.0    0.0| da0p3
    0      0      0      0    0.0      0      0    0.0    0.0| da0p4
    2     70     70  32499   20.4      0      0    0.0   93.0| gpt/raidz2disk6
    2    105    105  39799   16.2      0      0    0.0   92.8| gpt/raidz2disk0
    2    140    140  80511   13.9      0      0    0.0   99.1| gpt/raidz2disk5
    0      0      0      0    0.0      0      0    0.0    0.0| gpt/raidz2disk7
    2     53     53  18110   25.4      0      0    0.0   99.7| gpt/raidz2disk1
    2    105    105  58049   15.9      0      0    0.0  100.9| gpt/raidz2disk4
    0      0      0      0    0.0      0      0    0.0    0.0| gpt/gptboot
    0      0      0      0    0.0      0      0    0.0    0.0| ufs/embboot
    0      0      0      0    0.0      0      0    0.0    0.0| zvol/storage/virt-www
    0      0      0      0    0.0      0      0    0.0    0.0| gpt/data
    0      0      0      0    0.0      0      0    0.0    0.0| ufsid/5edbd33911af3812
    0      0      0      0    0.0      0      0    0.0    0.0| ufs/data
```
Once this resilver is complete, I will replace ada0. Stay tuned!