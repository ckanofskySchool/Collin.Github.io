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

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/DailyJournal/ObjectDetectVideo.mp4" type="video/mp4">
</video>

## Motor Control Test (RC Controller)

For early hardware tests, I drove the CIM motor using a RC plane receiver sending PWM to the motor controller. This validated motor wiring and safety setup before the full control stack was ready.

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/DailyJournal/Motor moving.mp4" type="video/mp4">
</video>

## Raspberry Pi + Seeed RP2040 Integration (1/13/2026 - 1/15/2026)

I connected the Pi's tracking output to the Seeed RP2040 so the microcontroller could translate tracking data into motor commands. This gave me the first usable end-to-end pipeline.

<img src="assets/img/portfolio/Electrical/dualMotorFullSetup.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/Electrical/AllMotorSubsytemWorking.mp4" type="video/mp4">
</video>

## Control Algorithms

### Deadzone Control (1/27/2026)

The first follow test used a deadzone method: if the target was outside a position/size range, the robot would set fixed motor powers until the target returned to center. This proved the full system worked, but speed and turning were static.

<!-- TODO: add video of first follow test -->
<img src="https://placehold.co/1200x675/png?text=First+Follow+Test+Video" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

### PID Control (1/28/2026 - 1/30/2026)

 In robotics, we use a method of controlling mechanisms called a PID Controller. The purpose of a PID Controlleris to compare where something is, to where it should be, and correct an appropriate amount to reach the desired destination. This proccess is continuously looped until the the desired destination is reached. Lets break down how the PID works and what PID stands for!

Full credit to RoboFTC for the Following PID Explanantion:
 
A **PID controller** is one of the most common control algorithms in robotics. It helps mechanisms reach and hold positions or velocities accurately by adjusting motor power based on feedback.

PIDF stands for:

- **P** — Proportional
- **I** — Integral
- **D** — Derivative

---

### 🔧 What is PID?

A PID controller constantly compares a **target value (setpoint)** to the **current value (measured)** and calculates how much power to apply.

#### Basic Formula:

```text
output = (P * error) + (I * accumulatedError) + (D * errorRate)
```

#### ✔️ Each term has a role:

- **Proportional (P)** — Corrects based on the current error. Bigger error = bigger correction.
- **Integral (I)** — Corrects accumulated past errors to eliminate drift or steady-state error.
- **Derivative (D)** — Predicts future error by reacting to how quickly the error is changing.

---

### 🧠 How PIDF Works (Step-by-Step)

 **1. Calculate Error**

```text
error = targetPosition - currentPosition
```

 **2. Compute Terms**

```text
P = kP * error
I = kI * totalAccumulatedError
D = kD * (error - lastError) / deltaTime
```

 **3. Calculate Output**

```text
output = P + I + D
```

 **4. Apply Output**

```text
setPower(output)
```

### How to Tune PID constants

1. Proportional (P) — Makes the mechanism respond to error. Raise it until it moves toward the target quickly but doesn’t overshoot too much.
2. Integral (I) — Use to eliminate small, constant errors that P can’t fix (e.g., due to friction). Be careful: too much I causes wind-up and instability.
3. Derivative (D) — Add if the mechanism overshoots or oscillates. D slows down the motion as it approaches the target.


### How I implemented PID in the Robopack

For the robopack program I tested today, I chose to only implement a P-Controller to start. The reason for this is due to the margin of error I am allowing which is a lot and the simplicity of a P-Controller. In the future I plan to implement I and D to get more precise motions but currenty am very content with the Robopack following ability. Below is a video of my tuned P-Controller on the robot.

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/DailyJournal/" type="video/mp4">
</video>


I later moved to a PID control loop for smoother tracking and more proportional motor response. After fixing a sign error in the angle output, I upgraded from P-only to full PID and verified motor directions.

<!-- TODO: add video of PID tuning test -->
<img src="https://placehold.co/1200x675/png?text=PID+Tuning+Video" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

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


