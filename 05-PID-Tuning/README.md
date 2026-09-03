# 05 - PID Tuning

## Goal
Tune from evidence, with limits enabled from the beginning.

1. Set `Ki = 0`, `Kd = 0`.
2. Increase `Kp` until response is fast but not persistently oscillating.
3. Add `Ki` slowly until steady-state error disappears.
4. Add small filtered `Kd` only when damping is needed.
5. Repeat across load, voltage, temperature, and direction.

Measure rise time, overshoot, settling time, steady-state error, command activity,
current peaks, and fault behavior. Compare equal step sizes and equal settling bands.

For anti-windup, stop integrating while output is saturated in the same direction
as the error, and allow integration when it would move output back from the limit.
Use gain scheduling only when one gain set cannot cover the operating range.

## Checkpoint

Make a gain and metrics table for at least three operating conditions. Select gains
from the worst acceptable condition, not the prettiest single plot.
