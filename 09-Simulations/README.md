# 09 - Simulations

## Goal
Find sign errors, unstable gains, saturation problems, and bad assumptions before hardware.

Start with a first-order plant and explicit sample time, actuator saturation,
sensor noise, measurement delay, and reference steps. Use the same discrete
controller equations and units as the embedded implementation.

Test reference steps, load disturbances, supply changes, noise, stale measurements,
saturation, startup, disable, and recovery. Plot reference, measurement, error,
unsaturated output, saturated output, and integral state.

## Checkpoint

Every fault scenario must end in a bounded, known output before a cautious bench test.
