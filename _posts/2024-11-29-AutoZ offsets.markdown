---
layout: post
title: "Calibrating Z-Offset with AutoZ (Calibrate_Z)"
date: 2024-11-29
categories: 3D Printing Klipper Calibration
---

The video *[Never adjust your 3D Printer Z Offset again with Klipper and a plugin](https://youtu.be/oQYHFecsTto?si=PoagrTXBOvhGXVYQ)* is a great resource for Z-offset calibration, but the macros for `auto_z` have changed since it was made. Therefore, these notes reflect the current process and configuration.

A good calibration model is essential for dialing in the Z-offset. I use the calibration model *[First Layer Test](https://www.thingiverse.com/thing:2177790)* and slice it with my normal settings. This process may also be necessary when changing components that could affect the Z-offset, such as cooling ducts or other modifications. For example, after installing a new tri horn with a built-in Klicky probe, I had to perform this calibration.

Once that’s done, I proceed with the following steps:

### 1. Console Output After Running `CALIBRATE_Z`
These results are shown in the console output after the `CALIBRATE_Z` macro has run (usually as part of `start_print`):  
- `current z axis position_endstop=0.475 - new offset=0.736250`  
- `bed_probe=6.944 - (switch=6.679 - nozzle=0.481 + switch_offset=0.010) --> new_offset=0.736250`.  
- Ignoring "POSSIBLE SUGGESTION" values for `position_endstop`.  

### 2. Adjusting `position_endstop`
Adjusting the `position_endstop`, which is defined in the `[stepper_z]` section of the configuration:  
- `position_endstop: 0.475` (derived from `CALIBRATE_Z`).  
- I get to this number by printing and manually adjusting the Z-offset. I start with +0.1 and gradually bring it down. I take note of the number and calculate the delta between the "new_offset" output and my manually adjusted value.  
- If the calculated `position_endstop` is out of range, I update the `autoz.cfg` file, particularly the `offset_margins` parameter, to ensure proper calibration.  

### 3. Fine-Tuning with `switch_offset`
Fine-tuning can be done with the `switch_offset` parameter in `autoz.cfg`. The lower the number, the higher the nozzle will be, and the higher the number, the closer the nozzle will be to the bed. A standard `switch_offset` is 0.5, and if it's too far from this value, I change the `position_endstop` accordingly.  

These steps ensure precise Z-axis positioning for consistent first-layer quality and overall print success.
