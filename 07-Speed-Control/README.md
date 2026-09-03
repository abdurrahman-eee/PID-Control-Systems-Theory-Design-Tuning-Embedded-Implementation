# 07 - Speed Control

## Goal
Control speed with a slower outer loop around a current or torque loop.

```text
speed_ref -> speed PI -> current_ref -> current PI -> motor
```

The current loop must settle substantially faster than the speed loop. Encoder
speed from position differences is noisy at low speed and quantized at short periods;
period measurement, filtering, or an observer may be better across a wide range.
Handle counter wraparound and signed direction explicitly.

Limit the speed controller output to a safe current reference. Apply acceleration
and deceleration ramps when mechanical shock matters, without hiding a poor tune.

## Checkpoint

Test no-load and loaded steps in both directions. Check current-limit interaction,
zero-speed behavior, reversal, and encoder timeout handling.
