# 01 - Control System Fundamentals

## Goal
Understand the closed loop before choosing a gain.

```text
reference -> error -> controller -> actuator -> plant -> sensor
                ^                                      |
                +-------------- feedback -------------+
```

- **Reference**: desired value.
- **Measurement**: sensor value.
- **Error**: `e = reference - measurement`.
- **Plant**: physical system.
- **Actuator**: device that changes the plant.

## Open and closed loop

`50% PWM -> motor` is open loop: it cannot know that voltage or load changed. A
closed loop measures the result and corrects the difference. Feedback improves
disturbance rejection, but adds delay, noise, and possible instability.

## Digital control

A microcontroller repeats this at a fixed period `Ts`:

```text
sample -> validate -> calculate -> saturate -> update actuator
```

Sampling frequency is `fs = 1 / Ts`. Record the real period and jitter; do not
assume the scheduler is perfect.

## Response vocabulary

- **Rise time**: time to approach the target.
- **Overshoot**: amount beyond the target.
- **Settling time**: time to remain inside a defined band, such as `+/- 2%`.
- **Steady-state error**: final target minus measurement.

## PWM and practical limits

PWM duty switches the power stage; coil resistance, inductance, freewheel path,
supply voltage, and switching frequency determine current. A first estimate is:

```text
duty ~= target_current * coil_resistance / supply_voltage
```

Always clamp output, reject impossible or stale measurements, define startup and
fault behavior, and prevent integral windup during saturation.

## Worked example

`target = 18 A`, `measured = 16 A`, therefore `error = 2 A`. With
`Kp = 0.04 duty/A`, proportional output is `0.08 duty`, or 8% duty. Feedback,
feed-forward, and saturation determine the final command.

## Checkpoint

Write down units, sensor rate, actuator limits, maximum expected error, and safe
output for every fault before moving to Chapter 02.
