# 03 - P, PI, and PID Control

## Goal
Choose the smallest controller that meets the response requirement.

```text
P:   u = Kp * e
PI:  u = Kp * e + Ki * integral(e)
PID: u = Kp * e + Ki * integral(e) + Kd * derivative(e)
```

- **P** reacts now. It is simple, but usually leaves offset.
- **I** remembers error and removes offset, but can wind up.
- **D** reacts to trend and adds damping, but exposes noise.

Start with P. Add I only when offset matters. Add D only when a measured,
filtered derivative solves a demonstrated overshoot problem. PI is often the
best current-loop default.

For a reference step, derivative on measurement avoids derivative kick:
`D = -Kd * d(measurement)/dt` for a positive-control loop.

## Checkpoint

Apply a small positive step unloaded. Confirm the measurement moves in the desired
direction, the command is bounded, and disable returns the actuator to safe output.
