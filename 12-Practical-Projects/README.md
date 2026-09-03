# 12 - Practical Projects

## Goal
Turn a tuned algorithm into a testable engineering system.

1. Define variable, units, limits, and acceptance metrics.
2. Instrument signals and identify the plant.
3. Simulate normal and fault cases.
4. Bring up hardware at low energy with independent protection.
5. Tune inner loops, then outer loops.
6. Test voltage, load, temperature, repeatability, and endurance.
7. Record firmware, gains, calibration, wiring, and evidence.

Suggested projects include E-clutch current control, cascaded motor control, a
thermal PI controller, and a CAN-controlled actuator. Use explicit states such as
`DISABLED`, `READY`, `ENGAGING`, `HOLDING`, `RELEASING`, and `FAULT`; a PID formula
alone does not decide whether movement is allowed.

## Final acceptance

The system tracks, remains bounded during disturbances, recovers from saturation,
rejects invalid inputs, reaches a safe state on timeout, and produces diagnostic logs.
