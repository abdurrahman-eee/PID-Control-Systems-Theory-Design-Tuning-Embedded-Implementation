# 02 - Derivative and Integral

## Goal
Understand the two time-based terms before using them in code.

- **Integral** is accumulated error: `integral += error * dt`.
- **Derivative** is rate of change: `derivative = (error - previous_error) / dt`.

The integral removes persistent offset but can wind up during actuator saturation.
Clamp it or use conditional integration. The derivative can add damping but also
amplifies measurement noise; filter it and consider derivative on measurement.

See [PID-Cheat-Sheet.md](PID-Cheat-Sheet.md) for the complete worked numerical
example and STM32-style discrete equations.

## Checkpoint

For a known error sequence, calculate the integral and derivative by hand, then
compare the values with a small unit test using the same `dt` and units.
