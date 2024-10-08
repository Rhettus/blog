---
layout: post
title: "ZFS Disk performance troubleshooting under FreeBSD"
date: 2021-06-20 19:32:20 +0300
description: # Add post description (optional)
img: zfs/zfs_cover.png # Add image post (optional)
categories: Tech
---

I have had some issues with my RAIDZ2 storage pool and general performance under XigmaNAS/FreeBSD. I was seeing issues with my VirtualBox VM which runs BlueIris security sotfware under a Windows 10 VM being generally laggy and unresponsive. I tried all the usual trick which write caching etc but it was still slow.

I thought I would try to track down my issues and document the process I go through here. 

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

Interestingly ada0 did not respond to this. A clue? I tried again later on ada0 and got the following seek times. The full stroke time is off the charts

```
Seek times:
        Full stroke:      250 iter in 104.906918 sec =  419.628 msec
        Half stroke:      250 iter in  10.341546 sec =   41.366 msec
        Quarter stroke:   500 iter in  20.552574 sec =   41.105 msec
        Short forward:    400 iter in   4.611131 sec =   11.528 msec
        Short backward:   400 iter in   6.014354 sec =   15.036 msec
        Seq outer:       2048 iter in   0.196127 sec =    0.096 msec
        Seq inner:       2048 iter in   0.164158 sec =    0.080 msec

Transfer rates:
        outside:       102400 kbytes in   0.717848 sec =   142649 kbytes/sec
        middle:        102400 kbytes in   1.523067 sec =    67233 kbytes/sec
        inside:        102400 kbytes in   1.546337 sec =    66221 kbytes/sec
```

## Use "gstat" to find which disk is busy

This is a great uility which really helped me hone in on which disk was holding up the resilver. I have been looking at the %busy column and found that my ada0 device was constantly at or over 100%. ada1 was the device being replaced during the resilver.

`gstat -I 10s`


```
dT: 10.006s  w: 10.000s
 L(q)  ops/s    r/s   kBps   ms/r    w/s   kBps   ms/w   %busy Name
    0      0      0      0    0.0      0      0    0.0    0.0| md0
    0      0      0      0    0.0      0      0    0.0    0.0| md1
    5    169    154   7372   15.9     14    133    0.2   99.1| ada0
    1    957      0      0    0.0    957   5103    0.4   37.4| ada1
    2    105     91   2713   12.7     14    118    0.1   45.2| ada2
    3    114    100   3152    9.2     14    127    0.4   42.1| ada3
    2    144    129   5910   11.6     14    124    0.3   65.7| ada4
    0     14      0      2   12.1     14    124    0.1    3.2| ada5
    2    148    133   5860   12.3     14    132    0.1   57.1| ada6
    2    153    137   6215   11.0     15    126    0.1   65.2| ada7
```
Once this resilver is complete, I will replace ada0. Stay tuned!