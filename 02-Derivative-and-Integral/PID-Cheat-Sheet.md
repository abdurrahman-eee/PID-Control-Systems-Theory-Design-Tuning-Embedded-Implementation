# PID Cheat Sheet (from the roots)

Goal: keep a system's **output** at a desired **setpoint** by constantly fixing the **error**.

```
error  e(t) = setpoint - measured_value
```

PID = **P**roportional + **I**ntegral + **D**erivative. Each term looks at the error from a different angle in time:

| Term | Looks at... | Answers |
|---|---|---|
| P | error **now** | "How wrong am I right now?" |
| I | error **history** (sum over time) | "How wrong have I been for a while?" |
| D | error **rate of change** | "How fast is the error changing?" |

---

## 1. Proportional (P) — quick recap

```
P_out = Kp * e(t)
```
Bigger error → bigger push. Alone, it always leaves a small steady-state error (offset) because as error shrinks, the push shrinks too — it can never fully "catch up."

---

## One example used for the whole sheet

So every number below lines up, every section (I, D, and the combined PID at the end) reuses **the same speed-control loop**:

```
dt = 0.1 s          (control loop runs every 0.1 s)
Kp = 3, Ki = 2, Kd = 0.5
e(t) sequence:  t=0.0 → e=6 (starting error, used as e_prev for the first row)
                t=0.1 → e=5
                t=0.2 → e=4
                t=0.3 → e=1   (error suddenly drops fast here)
```

---

## 2. Integral (I) — the memory term

### Why it exists
P alone leaves leftover error (steady-state error). Integral **accumulates** that leftover error over time and keeps adding push until the error is driven to zero.

### The formula (continuous)
```
I_out = Ki * ∫ e(t) dt        (0 to t)
```
`∫ e(t) dt` = the running **sum/area** under the error curve.

### Discrete (real code / embedded) form
Since a microcontroller can't do a true integral, it approximates it every loop tick:
```
integral_sum += e(t) * dt
I_out = Ki * integral_sum
```
- `dt` = time between control loop updates (e.g., 0.01 s for a 100 Hz loop)
- `integral_sum` keeps growing/shrinking as long as error exists

### Worked example
Using the example above (`dt = 0.1 s`, `Ki = 2`):

| Time | e(t) | integral_sum (running total) | I_out = Ki × sum |
|---|---|---|---|
| t=0.1s | 5 | 0 + 5×0.1 = 0.5 | 1.0 |
| t=0.2s | 4 | 0.5 + 4×0.1 = 0.9 | 1.8 |
| t=0.3s | 1 | 0.9 + 1×0.1 = 1.0 | 2.0 |

Even as error `e(t)` shrinks (5→4→1), `I_out` keeps **growing** (1.0→1.8→2.0), because it's summing everything so far. This is exactly what removes steady-state error.

### Watch out for: Integral Windup
If error stays large for a long time (e.g., motor physically blocked), `integral_sum` grows huge → causes big overshoot when the block clears. Fix = **clamp** the integral term:
```
integral_sum = clamp(integral_sum, -MAX, +MAX)
```

---

## 3. Derivative (D) — the prediction term

### Why it exists
D looks at **how fast error is changing** and pushes back against fast changes → it dampens oscillation/overshoot, acting like a brake.

### The formula (continuous)
```
D_out = Kd * d(e(t))/dt
```
`d(e(t))/dt` = the **slope** of the error curve right now.

### Discrete (real code) form
```
D_out = Kd * (e(t) - e_prev) / dt
```
- `e_prev` = error from the previous loop tick
- `(e(t) - e_prev)` = how much error changed this tick

### Worked example
Same loop as above (`dt = 0.1 s`, `Kd = 0.5`):

| Time | e(t) | e_prev | Δe = e - e_prev | D_out = Kd × Δe/dt |
|---|---|---|---|---|
| t=0.1s | 5 | 6 | 5-6 = -1 | 0.5×(-1/0.1) = -5 |
| t=0.2s | 4 | 5 | 4-5 = -1 | 0.5×(-1/0.1) = -5 |
| t=0.3s | 1 | 4 | 1-4 = -3 | 0.5×(-3/0.1) = -15 |

Notice at t=0.3s the error dropped **fast** (4→1) — D reacts strongly (-15) to slow that fast approach down, preventing overshoot past the setpoint.

### Watch out for: Derivative Kick / Noise
Derivative amplifies noise (it's a slope calc — tiny jitter in sensor readings → huge spikes). Fixes:
- Low-pass filter the D term
- Compute derivative **on measurement**, not on error (avoids kick when setpoint suddenly changes)

---

## 4. Putting it together: full PID formula

### Continuous
```
u(t) = Kp*e(t) + Ki*∫e(t)dt + Kd*de(t)/dt
```

### Discrete (what you actually code, e.g. on STM32)
```c
error = setpoint - measured;
integral_sum += error * dt;
integral_sum = clamp(integral_sum, -MAX_I, MAX_I);   // anti-windup
derivative = (error - prev_error) / dt;

output = Kp*error + Ki*integral_sum + Kd*derivative;

prev_error = error;
```

### Full numeric example
Same loop throughout (`Kp=3, Ki=2, Kd=0.5, dt=0.1s`). Take the row at **t = 0.3 s**, pulling the numbers straight from the I and D tables above:

```
e(t)          = 1      (from the e(t) sequence)
integral_sum  = 1.0    (from the Integral table, t=0.3s)
derivative    = (e - e_prev)/dt = (1-4)/0.1 = -30   (from the Derivative table, t=0.3s)
```

```
P_out = Kp × e            = 3 × 1     = 3
I_out = Ki × integral_sum = 2 × 1.0   = 2
D_out = Kd × derivative   = 0.5 × -30 = -15

u(t) = P_out + I_out + D_out = 3 + 2 - 15 = -10   ← final motor/actuator command
```

This negative output makes sense: at t=0.3s the error crashed from 4 down to 1 in one tick — the system is rushing toward the setpoint — so D dominates and pulls the command down hard to brake before it overshoots.

---

## 5. What each gain "feels like" (intuition table)

| Gain ↑ | Effect |
|---|---|
| Kp ↑ | Faster response, less steady-state error, but more overshoot/oscillation |
| Ki ↑ | Removes steady-state error, but slower, risk of overshoot & windup |
| Kd ↑ | Reduces overshoot, adds damping, but amplifies noise |

Quick tuning order (manual): set Ki=Kd=0 → raise Kp until reasonable response → add Ki to kill steady-state error → add small Kd to reduce overshoot.

---

## 6. One-line summary

- **P** = react to present error
- **I** = fix accumulated past error (removes offset, but can overshoot/wind up)
- **D** = anticipate future error from its current rate of change (damps oscillation, but noisy)

See `05-PID-Tuning/` for tuning methods (Ziegler-Nichols etc.) and `10-Embedded-Implementation/` for full C code on real hardware.
