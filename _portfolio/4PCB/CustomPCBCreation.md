---
title: Custom PCB Creation
subtitle: Creating a double sided PCB board for motor control
image: assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg
alt: 

caption:
  title: Custom PCB Creation
  subtitle: Creating a double sided PCB board for motor control
  thumbnail: assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg
---

[PCB Board Design Files](assets/Files/Electrical/SeeedControlBoard.zip)
[PCB Board CAM files](assets/Files/Electrical/SeeedControlBoard-F_Cu.zip)

## Overview

I designed and milled a double-sided PCB to mount the Seeed RP2040 and handle motor control connections. This was my first time milling a two-sided board, so alignment and toolpath depth were the main challenges.

## Milling Challenges and Fixes (From Daily Journal)

- 10/28/2025: Learned MakeraCAM and prepared CAM files for a two-sided board.
- 10/29/2025: First milling attempt failed due to flip alignment and auto-leveling depth issues.
- 11/6/2025: Second attempt used a 3D printed jig and cut successfully, but still had a hollowed area.
- 11/7/2025: Tried to connect both sides with solder only and damaged traces.
- 11/8/2025: Remilled and used pin headers through holes, then soldered and trimmed. This worked cleanly.

<!-- TODO: add photo of the PCB mounted in the Makera or on the milling bed -->
<img src="https://placehold.co/1200x675/png?text=PCB+Milling+Setup" width="1080" height="720" style="display:block; margin:0 auto;">

<img src="assets/img/portfolio/DailyJournal/SeeedBackPinsPlaced.jpg" width="1080" height="720" style="display:block; margin:0 auto;">
<img src="assets/img/portfolio/DailyJournal/SeeedFrontPinsSoldered.jpg" width="1080" height="720" style="display:block; margin:0 auto;">
<img src="assets/img/portfolio/DailyJournal/SeeedFrontSoldered.jpg" width="1080" height="720" style="display:block; margin:0 auto;">
<img src="assets/img/portfolio/DailyJournal/SeeedBackSoldered.jpg" width="1080" height="720" style="display:block; margin:0 auto;">
<!-- TODO: add photo of the finished PCB installed on the robot or test bench -->
<img src="https://placehold.co/1200x675/png?text=Installed+PCB" width="1080" height="720" style="display:block; margin:0 auto;">




