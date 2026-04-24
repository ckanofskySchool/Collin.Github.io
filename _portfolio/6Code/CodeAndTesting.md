---
title: Code & Testing
subtitle: Software & Testing for the Robopack
image: assets/img/portfolio/DailyJournal/ObjectDetectCover.png
alt: Object detection running on Raspberry Pi

caption:
  title: Code & Testing
  subtitle: Software & Testing
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

## Raspberry Pi + Seeed RP2040 Integration (1/13/2026 – 1/15/2026)

I connected the Pi's tracking output to the Seeed RP2040 so the microcontroller could translate tracking data into motor commands. This gave me the first usable end-to-end pipeline.

<img src="assets/img/portfolio/Electrical/dualMotorFullSetup.jpg" style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;">

<video style="display:block; margin:0 auto; width:100%; max-width:1080px; height:auto;" controls>
  <source src="assets/img/portfolio/Electrical/AllMotorSubsytemWorking.mp4" type="video/mp4">
</video>

# Seeed Xiao RP2040 Code — Control Algorithm

### Deadzone Control (1/27/2026)

The first follow test used a deadzone method: if the target was outside a position/size range, the robot would set fixed motor powers until the target returned to center. This proved the full system worked, but speed and turning were static and the movements were not fluid at all.

### PID Control (1/28/2026 – 1/30/2026)

In robotics, we use a method of controlling mechanisms called a PID Controller. The purpose of a PID Controller is to compare where something is, to where it should be, and correct an appropriate amount to reach the desired destination. This process is continuously looped until the desired destination is reached. Let's break down how the PID works and what PID stands for!

Full credit to RoboFTC for the following PID explanation:
 
A **PID controller** is one of the most common control algorithms in robotics. It helps mechanisms reach and hold positions or velocities accurately by adjusting motor power based on feedback.

PID stands for:

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

### 🧠 How PID Works (Step-by-Step)

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

### How to Tune PID Constants

1. Proportional (P) — Makes the mechanism respond to error. Raise it until it moves toward the target quickly but doesn't overshoot too much.
2. Integral (I) — Use to eliminate small, constant errors that P can't fix (e.g., due to friction). Be careful: too much I causes wind-up and instability.
3. Derivative (D) — Add if the mechanism overshoots or oscillates. D slows down the motion as it approaches the target.


### How I Implemented PID in the Robopack

For the Robopack, I implemented a **Gain-Scheduled PD Controller** — a PD controller that smoothly transitions between two sets of tuning constants based on the robot's current speed. At low speeds, the robot uses more aggressive turning gains to stay responsive. At high speeds, the gains are reduced to prevent oscillation and overshooting.

I also use **Derivative-on-Measurement** instead of Derivative-on-Error to prevent "derivative kick" — sudden spikes in the D term when the target changes abruptly. Additionally, a **Proportional Desaturation** strategy ensures that when one motor would exceed its limits, both motors are scaled proportionally rather than just clipping one, preserving the intended turn ratio.

## Safety Considerations

During testing, the robot briefly accelerated toward a person due to a large error spike. To solve the safety issue, I re-worked the code to do a complete stop when camera feed is blocked or target is lost. However, this brought a whole host of problems with the robot spontaneously stopping mid-movement, so I had to tune the sensitivity of the change detection and implement a grace period before the safety stop triggers. The robot now waits 25 frames (~0.5 seconds) of lost target before stopping, which prevents false triggers while still catching genuine safety scenarios.

---

# Final Code

## Seeed RP2040 Motor Controller Code

This is the final Arduino code running on the Seeed Xiao RP2040 that handles PID control of the drive motors:

```cpp
#include <Servo.h>

Servo LeftMotorPWM;
Servo RightMotorPWM;

#define LeftMotorLED D1
#define RightMotorLED D9

String cmd = "";
int LeftMotorPower = 90;
int RightMotorPower = 90;

int TargetAngle = 0;
int DetectedAngle;

int TargetDistance = 2200;
int DetectedDistance;

bool NewCmd = false;

unsigned long lastValidSerialTime = 0;

// ============================================================================
// PD PARAMETERS - TUNE THESE FOR YOUR ROBOT
// ============================================================================

// Angle PD gains (Normal / Low Speed)
float angleKp = 1.125;    // Proportional gain for angle
float angleKd = 0.15;     // Derivative gain for angle

// Angle PD gains (High Speed)
float highAngleKp = 0.5;  // Usually lower at high speeds to prevent oscillation
float highAngleKd = 0.2;  // Usually higher at high speeds to damp fast movements

// Speed thresholds for smooth PD transition
float transitionSpeedLow = 10.0;  // Speed where transition begins
float transitionSpeedHigh = 25.0; // Speed where transition ends

// Distance PD gains
float distanceKp = 0.03;   // Proportional gain for distance
float distanceKd = 0.0015; // Derivative gain for distance

// PD state variables
int lastDetectedAngle = 0;
int lastDetectedDistance = 0;
unsigned long lastComputeTime = 0;

// ============================================================================

void setup() {
  pinMode(LeftMotorLED, OUTPUT);
  pinMode(RightMotorLED, OUTPUT);

  LeftMotorPWM.attach(D10);
  RightMotorPWM.attach(D0);

  LeftMotorPWM.write(90);
  RightMotorPWM.write(90);

  Serial.begin(9600);
  Serial.setTimeout(10);
  lastComputeTime = millis();
}

void loop() {
  readInput();
  setPower(LeftMotorPower, RightMotorPower);
}

void readInput() {
  if (!Serial.available()) {
    // Failsafe: If no command in 300ms, STOP
    if (millis() - lastValidSerialTime > 300) {
      LeftMotorPower = 90;
      RightMotorPower = 90;
      resetPD();
    }
    return;
  }

  lastValidSerialTime = millis();
  String line = Serial.readStringUntil('\n');
  cmd = line;

  if (cmd.indexOf('M') >= 0) ManualCommands();
  if (cmd.indexOf('A') >= 0) AutoCommands();

  if (NewCmd) {
    Serial.println("R" + String(RightMotorPower) + " L" + String(LeftMotorPower));
    NewCmd = false;
  }
}

void ManualCommands() {
  if (cmd.indexOf('M') >= 0) {
    NewCmd = true;
    int tempRight = 90;
    int tempLeft = 90;
    sscanf(cmd.c_str(), "M R%d L%d", &tempRight, &tempLeft);

    RightMotorPower = tempRight;
    LeftMotorPower  = tempLeft;
    LeftMotorPower = constrain(LeftMotorPower, 90 - 50, 90 + 50);
    RightMotorPower = constrain(RightMotorPower, 90 - 50, 90 + 50);
    resetPD();
  }
}

void AutoCommands() {
  if (cmd.indexOf('A') >= 0) {
    NewCmd = true;
    sscanf(cmd.c_str(), "A T%d D%d", &DetectedAngle, &DetectedDistance);

    int angleError = DetectedAngle - TargetAngle;
    int distanceError = DetectedDistance - TargetDistance;

    unsigned long currentTime = millis();
    float dt = (currentTime - lastComputeTime) / 1000.0;
    
    if (dt > 0) {
      // 1. DISTANCE PD (Derivative on Measurement)
      float distanceP = distanceError * distanceKp;
      float distanceDerivative = (DetectedDistance - lastDetectedDistance) / dt;
      float distanceD = distanceDerivative * distanceKd;
      float speedCorrection = distanceP + distanceD;

      // 2. ANGLE PD (Gain-Scheduled with smooth interpolation)
      float currentSpeed = abs(speedCorrection);
      float t = (currentSpeed - transitionSpeedLow) / (transitionSpeedHigh - transitionSpeedLow);
      t = constrain(t, 0.0, 1.0); 

      float currentAngleKp = angleKp + t * (highAngleKp - angleKp);
      float currentAngleKd = angleKd + t * (highAngleKd - angleKd);

      float angleP = angleError * currentAngleKp;
      float angleDerivative = (DetectedAngle - lastDetectedAngle) / dt;
      float angleD = angleDerivative * currentAngleKd;
      float turnCorrection = angleP + angleD;
      
      // Update state
      lastDetectedDistance = DetectedDistance;
      lastDetectedAngle = DetectedAngle;
      lastComputeTime = currentTime;

      // 3. COMBINE with Proportional Desaturation
      float left_raw_effort = speedCorrection + turnCorrection;
      float right_raw_effort = speedCorrection - turnCorrection;

      float max_effort = max(abs(left_raw_effort), abs(right_raw_effort));
      float max_allowed = 60.0;

      if (max_effort > max_allowed) {
          left_raw_effort = (left_raw_effort / max_effort) * max_allowed;
          right_raw_effort = (right_raw_effort / max_effort) * max_allowed;
      }

      LeftMotorPower = 90 + round(left_raw_effort);
      RightMotorPower = 90 + round(right_raw_effort);
      LeftMotorPower = constrain(LeftMotorPower, 30, 150); 
      RightMotorPower = constrain(RightMotorPower, 30, 150);
    }
  }
}

void resetPD() {
  lastDetectedAngle = DetectedAngle;
  lastDetectedDistance = DetectedDistance;
  lastComputeTime = millis();
}

void setPower(int LP, int RP){
  RP = 180 - RP;
  LP = constrain(LP, 0, 180);
  RP = constrain(RP, 0, 180);

  LeftMotorPWM.write(LP);
  RightMotorPWM.write(RP);

  if(LP != 90){
    digitalWrite(LeftMotorLED, HIGH);
  } else {
    digitalWrite(LeftMotorLED, LOW);
  }
  if(RP != 90){
    digitalWrite(RightMotorLED, HIGH);
  } else {
    digitalWrite(RightMotorLED, LOW);
  }
}
```

## Raspberry Pi Vision Tracking Code

<!-- PASTE YOUR FINAL RASPBERRY PI CODE HERE -->
<!-- The code below is a placeholder - replace with your current running version -->

*Code will be added here — the Raspberry Pi runs a modified version of the `rpicam-hello` object detection demo with:*
- *A custom `SimpleTracker` class for IoU-based person tracking across frames*
- *EMA (Exponential Moving Average) smoothing on angle and distance outputs*
- *Target lock and re-acquisition logic to follow a specific person even when others enter the frame*
- *Serial output in the format `A T±XX D####` (angle in degrees, distance in mm)*
- *Safety stop when the target is lost for more than 25 frames*
- *CSV data logging for PID tuning analysis*

[View the full Robopack code repository on GitHub](https://github.com/Ckanofsky/Robopack)
