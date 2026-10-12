# Week 4 — Line Following in TRIK Studio

Two line-following programs for a TRIK robot: a simple on/off zigzag and a proportional (P) controller with normalized error.

## Setup

- A4 — left sensor
- A3 — right sensor
- A2 — single sensor (zigzag program only)
- M3 — left motor
- M4 — right motor

---

## 1. alongTheBigLine.qrs — Zigzag (on/off)

```
if A2 > 50:
    one motor = 10, other motor = 30
else:
    swap the two speeds
wait 100 ms
```

### Result

The robot zigzags along the edge of the line. It is simple and reliable, but slow and wobbly.

---

## 2. line_real_robot.qrs — P Controller with Normalized Error

### Controller

```
error = (A4 - left_reference) - (A3 - right_reference)

maxE  = max(maxE, |error|)
e     = error / maxE            // normalized to -1..1
u     = K * e

speed = base_speed - s * |u|    // slow down in turns

M3 = speed + u
M4 = speed - u
```

### Final Parameters

- K = 50
- Base speed = 25
- Turn slowdown s = 0.3
- Initial maxE = 20
- Control interval = 10 ms

The initial sensor readings are stored as reference values, and the robot then waits 1 s before starting.
The error is divided by the largest error seen so far, so the controller adapts to the sensors' actual range.
The base speed drops in sharp turns to keep the robot from overshooting the line.

### Result

The controller continuously changes the speed difference between the left and right motors to keep the robot centered on the line. It moves faster and more smoothly than the zigzag version.
