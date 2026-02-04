---
title: Code & Testing
subtitle: Coding & Testing codes for the Robopack
image: assets/img/portfolio/DailyJournal/ObjectDetectCover.png
alt: 

caption:
  title: Code & Testing
  subtitle: Coding & Testing codes for the Robopack
  thumbnail: assets/img/portfolio/DailyJournal/ObjectDetectCover.png
---

# Raspberry Pi Coding

## Vision Tracking (Raspberry Pi + AI Camera)

I started with the official `rpicam-hello` object detection demo and modified it to output bounding box center and size over serial. I added a stabilization filter so small jitter in the bounding box doesn't cause constant micro-adjustments.

<video width="854" height="480" controls>
  <source src="assets/img/portfolio/DailyJournal/ObjectDetectVideo.mp4" type="video/mp4">
</video>

## Motor Control Test (RC Controller)

For early hardware tests, I drove the CIM motor using a RC plane receiver sending PWM to the motor controller. This validated motor wiring and safety setup before the full control stack was ready.

<video width="854" height="480" controls>
  <source src="assets/img/portfolio/DailyJournal/Motor moving.mp4" type="video/mp4">
</video>

## Raspberry Pi + Seeed RP2040 Integration (1/13/2026 - 1/15/2026)

I connected the Pi’s tracking output to the Seeed RP2040 so the microcontroller could translate tracking data into motor commands. This gave me the first usable end-to-end pipeline.

<img src="assets/img/portfolio/Electrical/dualMotorFullSetup.jpg" width="854" height="480">

<video width="854" height="480" controls>
  <source src="assets/img/portfolio/Electrical/AllMotorSubsytemWorking.mp4" type="video/mp4">
</video>

## Control Algorithms

### Deadzone Control (1/27/2026)

The first follow test used a deadzone method: if the target was outside a position/size range, the robot would set fixed motor powers until the target returned to center. This proved the full system worked, but speed and turning were static.

<!-- TODO: add video of first follow test -->
<img src="https://placehold.co/1200x675/png?text=First+Follow+Test+Video" width="854" height="480">

### PID Control (1/28/2026 - 1/30/2026)

I moved to a PID control loop for smoother tracking and more proportional motor response. After fixing a sign error in the angle output, I upgraded from P-only to full PID and verified motor directions.

<!-- TODO: add video of PID tuning test -->
<img src="https://placehold.co/1200x675/png?text=PID+Tuning+Video" width="854" height="480">

## Safety Considerations

During testing, the robot briefly accelerated toward a person due to a large error spike. Planned safety improvements:
- Soft bumper to protect people and the frame
- Motor power rate limits to prevent sudden spikes
- Front limit switch to cut power on impact

## NOTES

10,000 Changes in area mean significant change
0-600 T value(angle)

Target Values for Mid:
T380
D90,000
