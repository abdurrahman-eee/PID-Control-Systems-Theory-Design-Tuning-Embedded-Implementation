# 10 - Embedded Implementation

## Goal
Implement deterministic control code that remains safe when conditions are not ideal.

```c
error = reference - measurement;
provisional = kp * error + ki * integral + kd * derivative;
output = clamp(provisional, output_min, output_max);
```

Use a measured or guaranteed `dt`. Keep state explicit, initialize it on enable,
and define reset behavior. Watch overflow, NaN/Inf, unit conversion, signedness,
and timer wraparound. Document filter delay.

Validate inputs before updating state. On timeout, invalid measurement, over-current,
or emergency stop, force the defined safe output. Unit-test zero error, signs,
saturation, recovery, `dt` variation, reset, invalid input, and fault transitions.
