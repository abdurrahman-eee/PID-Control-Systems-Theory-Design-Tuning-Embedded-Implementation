# 08 - Position Control

## Goal
Reach a target without instability, excessive force, or mechanical shock.

```text
position error -> position P -> speed_ref -> speed PI -> current PI -> motor
```

Position control commonly uses P at the outermost loop. Integral can fight hard
against stops and backlash, so add it only with strong anti-windup and a requirement.

Backlash, compliance, hard stops, friction, encoder resolution, and cable limits
can dominate response. Use soft limits, homing, velocity limits, and current or
force limits. A model cannot remove a mechanical constraint.

## Checkpoint

Perform small moves near the center and both limits. Verify holding, overshoot,
stall behavior, encoder direction, and recovery after restart.
