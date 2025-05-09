---
title: Klicky Probe - Mercury One 
description: Adding a clicky probe to converted Ender 5 Pro, Eva 2.4, Octopus 1.1 + CRTouch
date: 2024-11-04T18:43:24.954Z
img: 
tags: [3d Printing]
layout: post
---

In order for me to do auto Z leveling so I can actually get consistent first layers, I needed to replace my CRTouch probe with a klicky probe, or any probe which can push down another probe. There are serveral advantages in doing this anyway including a lighter toolhead, more travel room and last but not least getting rid of that rattle box.

I travelled the internet looking for a complete guide on installing a klicky probe on my EVA 2.4 toolhead and could not find one, only parts, and not specific to my printer. So this post will track my progress on installing a klicky probe to my Mercury One (Ender 5 Pro with Octopus 1.1 MCU)

Full credits go to `turtlecrawler` for creating the parts and doing the initial write up on discord.

## BOM (Build of materials)
These are the parts you will need on hand to build a klicky probe

| Quantity | Part Description                              |
|----------|-----------------------------------------------|
| 8x       | 3x6mm N52 magnets                             |
| 1x       | CA glue (Super Glue)                          |
| 1x       | D2F-5 or D2F-5L micro switches (Remove lever) |
| 2x       | M3 heatset inserts                            |
| 2x       | M2x10 self-tapping screws                    |
| 2x       | M3x20 Socket Head Cap Screws                 |
| 2x       | M5x10 Socket Head Cap Screws                 |
| 2x       | M5 Roll-in spring T-nuts                     |
| 1x       | 22-24 AWG wire                                |
| 1x       | JST-SM or Microfit 3.0 connectors            |

## Printed parts
All parts should be printed in ASA or ABS. 

All parts were downloaded from the ZeroG discorder server under the "Community" channel. Download the "[Klicky_-_Trihorn_Ducxt_-_Merc.zip](https://discord.com/channels/747612067951018075/1010991671426682951)" 

I'm also running the Revo hotend, so printed the Narrow/Default trihorn.

All parts should be printed in ASA or ABS. 

Since I'm running an old Ender 5 Pro with the original 235x235 bed I used the 65mm dock.

## Assembly

Drill out the probe mount where the microswitch fits in with a 1/16th drill bit so the legs can slide through.

The microswitch only uses 2 pins

## Klipper Configuration
Create a new directory in mainsail or fluidd and name it klicky

Upload all config files from the klicky zip into the klicky directory

Add `[include klicky/klicky-probe.cfg]` to your printer.cfg

Comment out or remove your current `[safe_z_home]` section, The klicky cfg includes a new safe z home.

If switching from a BLtouch, remove the `[bltouch]` z_offset from the save_config section.

```
[stepper_z]
endstop_pin: probe:z_virtual_endstop
position_min: -15.0
position_max: 300
homing_speed: 12 ### Currently running my first homing speed at 20 ###
second_homing_speed: 5
```

```
[probe]
pin: ^PG10 ### Check your board pinout, this is an example pin ###
x_offset: -2
y_offset: 28.75
z_offset: 0
speed: 5 ### I have found it accurate on my setup running as fast as 10, at 16 the accuracy started to degrade ###
samples:1 ### Klicky is accurate enough for a single sample, the remaining lines are not needed if you run a single sample ###
samples_result: median
sample_retract_dist: 1.0
samples_tolerance: 0.02
samples_tolerance_retries: 3
```

You only need to edit `klicky-probe.cfg` and `klicky-variables.cfg`
You should only have to adjust the dock location, bed size, and tool head park location.


## References

[Mercury One discord](https://discord.com/channels/747612067951018075/1010991671426682951)
[Octocpus 1.1 pinouts](https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-V1.0/blob/master/Hardware/BIGTREETECH%20Octopus%20-%20PIN.pdf)
[Klicky Github](https://github.com/jlas1/Klicky-Probe/tree/main/Printers/ZeroG/Mercury_One)
[ModBotArmy Video](https://www.youtube.com/live/UJE6YU_ReE0?si=r3STdD-Wx29HAjpB) - Warning 2.5 hours