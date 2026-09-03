# 11 - STM32 Examples

## Goal
Map the design onto STM32 peripherals without losing timing discipline.

```text
TIM trigger -> ADC/DMA -> scale/validate -> control update -> PWM compare
                                      |
                                      +-> CAN status and logging
```

Use a timer as the timing authority. Keep the high-rate current update synchronized
to ADC/PWM. Run speed, position, diagnostics, and telemetry at explicit slower rates.
Avoid blocking calls, heap allocation, long logging, and unbounded queues in the
critical path. Measure loop jitter and ADC-to-PWM latency on the target.

## Bring-up order

1. Verify clocks, timer frequency, ADC scaling, PWM polarity, and safe disable.
2. Test bounded fixed output.
3. Close current loop at low energy.
4. Add speed, position, CAN, and fault recovery one layer at a time.
