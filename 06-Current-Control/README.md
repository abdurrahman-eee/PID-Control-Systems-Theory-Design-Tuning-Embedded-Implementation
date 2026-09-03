# 06 - Current Control

## Goal
Regulate actuator current quickly and safely.

Current is closely related to motor torque and coil force, and normally responds
faster than speed or position. A fast inner current loop makes outer loops predictable.

```text
current_ref - current_measured -> PI -> duty -> power stage
```

For a coil, start with `duty_ff = Iref * R / Vbat`, then add PI feedback for
resistance, voltage, temperature, and dynamic errors.

Synchronize ADC sampling to PWM where possible, and choose the period from the
measured electrical time constant, noise, and CPU budget. Enforce peak-current
limits, sensor plausibility, startup blanking, and a gate-disable path.

## Checkpoint

On a current-limited fixture, verify tracking, ripple, saturation recovery, and
behavior after sensor disconnect before connecting mechanical load.
