# 04 - System Modeling

## Goal
Build a model simple enough to calculate and accurate enough to guide design.

A common starting point is the first-order plant:

```text
y(s) / u(s) = K / (tau*s + 1)
```

`K` is steady-state gain. `tau` is the time constant; after one `tau`, a step
response reaches about 63.2% of its final change.

For a DC motor:

```text
V = R*i + L*di/dt + Ke*omega
J*domega/dt = Kt*i - load_torque - friction
```

Current is normally faster than speed, which motivates cascaded current, speed,
and position loops.

## Identify from data

Record time, command, measurement, and supply. Apply a small safe step, estimate
`K` from the final change, then find the 63.2% crossing for `tau`. Repeat at
several loads and voltages because real plants are nonlinear.

Include ADC filtering, scheduling, PWM timing, communication, and actuator delay.
Delay reduces phase margin even when the plant equation looks correct.

## Checkpoint

Fit a first-order model to one step and compare it with a second step. Investigate
friction, delay, saturation, and operating-point dependence before adding model
complexity.
