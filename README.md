# PID Control Systems

## Theory, design, tuning, and embedded implementation

This is a practical course for moving from **"what is feedback?"** to a
reliable controller running on an STM32. Every chapter follows the same rhythm:

1. **Understand** the physical idea in plain language.
2. **Model** it with the smallest useful equation.
3. **Calculate** a worked example.
4. **Implement** it in discrete time.
5. **Verify** it with measurements, plots, and limits.

The examples use motors, coils, current loops, speed loops, and position loops.
They are also applicable to temperature, pressure, flow, and other feedback
systems.

## Learning path

| Chapter | Outcome |
|---|---|
| [01 Fundamentals](01-Fundamentals/README.md) | Understand feedback, error, sampling, PWM, and limits |
| [02 Derivative and Integral](02-Derivative-and-Integral/README.md) | Build the discrete-time math behind I and D |
| [03 P, PI, and PID](03-P-PI-PID-Control/README.md) | Choose the right controller structure |
| [04 System Modeling](04-System-Modeling/README.md) | Turn a physical plant into a useful model |
| [05 PID Tuning](05-PID-Tuning/README.md) | Tune gains from response data |
| [06 Current Control](06-Current-Control/README.md) | Design a fast electrical current loop |
| [07 Speed Control](07-Speed-Control/README.md) | Add a slower outer speed loop |
| [08 Position Control](08-Position-Control/README.md) | Control position without fighting the mechanics |
| [09 Simulations](09-Simulations/README.md) | Test behavior before connecting hardware |
| [10 Embedded Implementation](10-Embedded-Implementation/README.md) | Write deterministic, protected controller code |
| [11 STM32 Examples](11-STM32-Examples/README.md) | Map the design onto timers, ADC, CAN, and FreeRTOS |
| [12 Practical Projects](12-Practical-Projects/README.md) | Build, instrument, tune, and validate complete systems |

## A compact mental model

```text
setpoint -> compare -> controller -> actuator -> plant -> sensor
                ^                                      |
                +-------------- measured value -------+
```

The central rule is simple:

> **Measure -> compare -> correct -> limit -> repeat**

## Prerequisites

- Basic algebra and graphs
- Familiarity with C and a microcontroller timer interrupt
- A way to record time-stamped setpoint, measurement, output, and fault data

## Engineering conventions

- Use SI units internally: seconds, amperes, volts, radians, and newtons.
- Keep the control period fixed and measure it rather than assuming it.
- Saturate outputs deliberately and design anti-windup with the actuator limit.
- Treat sensor plausibility, startup, timeout, and fault behavior as part of the controller.
- Tune on a current-limited test setup before applying full mechanical load.

## Repository status

The chapters are being developed in order. Chapter 01 is the foundation; each
later chapter adds one layer of theory and one layer of implementation detail.

---

# 2. Derivative | ডেরিভেটিভ

## 2.1 Constant Rule

### Formula

**d/dx (C) = 0**

### Use when

The term contains no `x`.

### Example

**d/dx (10) = 0**

### Why?

`10` does not change when `x` changes.

---

## 2.2 Power Rule

### Formula

**d/dx (x<sup>n</sup>) = n x<sup>n−1</sup>**

### Use when

The term looks like:

**a x<sup>n</sup>**

### Memory

> **Power সামনে আনো → power থেকে 1 কমাও**

### Example

Find the derivative of:

**10x<sup>3</sup>**

Step 1 — Bring the power `3` in front:

**10 × 3x<sup>3−1</sup>**

Step 2 — Reduce the power:

**30x<sup>2</sup>**

### Answer

**y' = 30x<sup>2</sup>**

---

## 2.3 Sum / Difference Rule

### Formula idea

Differentiate every term separately.

### Example

**y = 5x<sup>4</sup> + 3x<sup>3</sup> − 7x<sup>2</sup> + 6x + 10**

Term by term:

| Original | Derivative |
|---|---|
| 5x<sup>4</sup> | 20x<sup>3</sup> |
| 3x<sup>3</sup> | 9x<sup>2</sup> |
| −7x<sup>2</sup> | −14x |
| 6x | 6 |
| 10 | 0 |

### Answer

**y' = 20x<sup>3</sup> + 9x<sup>2</sup> − 14x + 6**

---

## 2.4 Product Rule

Use when **two functions are multiplied**.

### Form

**y = u × v**

### Formula

**y' = u'v + uv'**

### Memory

> **First derivative × Second + First × Second derivative**

### Example

**y = (2x<sup>2</sup> + 3)(x<sup>3</sup> − 4)**

Let:

- **u = 2x<sup>2</sup> + 3**
- **v = x<sup>3</sup> − 4**

Then:

- **u' = 4x**
- **v' = 3x<sup>2</sup>**

Apply Product Rule:

**y' = (4x)(x<sup>3</sup> − 4) + (2x<sup>2</sup> + 3)(3x<sup>2</sup>)**

Expand:

**y' = 4x<sup>4</sup> − 16x + 6x<sup>4</sup> + 9x<sup>2</sup>**

### Answer

**y' = 10x<sup>4</sup> + 9x<sup>2</sup> − 16x**

---

## 2.5 Quotient Rule

Use when **one function is divided by another**.

### Form

**y = u / v**

### Formula

**y' = (u'v − uv') / v<sup>2</sup>**

### Memory

> **Low × D-High − High × D-Low, all over Low²**

### Example

**y = (x<sup>2</sup> + 3) / (x + 1)**

Let:

- **u = x<sup>2</sup> + 3**
- **v = x + 1**

Then:

- **u' = 2x**
- **v' = 1**

Apply Quotient Rule:

**y' = [(2x)(x + 1) − (x<sup>2</sup> + 3)(1)] / (x + 1)<sup>2</sup>**

Simplify numerator:

**2x<sup>2</sup> + 2x − x<sup>2</sup> − 3**

= **x<sup>2</sup> + 2x − 3**

### Answer

**y' = (x<sup>2</sup> + 2x − 3) / (x + 1)<sup>2</sup>**

---

## 2.6 Chain Rule

Use when there is a **function inside another function**.

Typical shapes:

- `(inside)<sup>n</sup>`
- `sin(inside)`
- `cos(inside)`
- `e<sup>inside</sup>`

### Formula idea

**Derivative = Outside derivative × Inside derivative**

### Memory

> **বাইরের derivative করো → ভিতর same রাখো → ভিতরের derivative দিয়ে multiply করো**

### Example

**y = (3x<sup>2</sup> + 2)<sup>5</sup>**

Outer derivative:

**5(3x<sup>2</sup> + 2)<sup>4</sup>**

Inner derivative:

**d/dx (3x<sup>2</sup> + 2) = 6x**

Multiply:

**y' = 5(3x<sup>2</sup> + 2)<sup>4</sup> × 6x**

### Answer

**y' = 30x(3x<sup>2</sup> + 2)<sup>4</sup>**

---

# 3. Common Derivatives | সাধারণ সূত্র

| Function | Derivative |
|---|---|
| C | 0 |
| x<sup>n</sup> | n x<sup>n−1</sup> |
| sin x | cos x |
| cos x | −sin x |
| tan x | sec<sup>2</sup>x |
| e<sup>x</sup> | e<sup>x</sup> |
| ln x | 1/x |

### Example: Chain Rule with sin

**y = sin(x<sup>2</sup>)**

Outside derivative:

**cos(x<sup>2</sup>)**

Inside derivative:

**2x**

### Answer

**y' = 2x cos(x<sup>2</sup>)**

---

# 4. Which Derivative Rule Should I Use?

| What you see | Use |
|---|---|
| a x<sup>n</sup> | **Power Rule** |
| f(x) + g(x) | **Term-by-term** |
| f(x) × g(x) | **Product Rule** |
| f(x) / g(x) | **Quotient Rule** |
| f(g(x)) | **Chain Rule** |

> A real problem can require **more than one rule at the same time**.

---

# 5. Complete Mixed Derivative Example

Find the derivative of:

### **y = [x<sup>2</sup>(3x + 1)<sup>4</sup>] / (x + 2)**

This uses:

- Power Rule
- Product Rule
- Chain Rule
- Quotient Rule

---

## Step 1 — Separate numerator and denominator

Let:

**u = x<sup>2</sup>(3x + 1)<sup>4</sup>**

**v = x + 2**

Therefore:

**y = u / v**

---

## Step 2 — Find u'

Because `u` is a product:

**u = x<sup>2</sup> × (3x + 1)<sup>4</sup>**

Let:

- **a = x<sup>2</sup>**
- **b = (3x + 1)<sup>4</sup>**

Then:

**a' = 2x**

For `b`, use Chain Rule:

**b' = 4(3x + 1)<sup>3</sup> × 3**

So:

**b' = 12(3x + 1)<sup>3</sup>**

Now Product Rule:

**u' = a'b + ab'**

So:

**u' = 2x(3x + 1)<sup>4</sup> + 12x<sup>2</sup>(3x + 1)<sup>3</sup>**

Factor common terms:

**u' = 2x(3x + 1)<sup>3</sup>[(3x + 1) + 6x]**

Therefore:

### **u' = 2x(3x + 1)<sup>3</sup>(9x + 1)**

---

## Step 3 — Find v'

**v = x + 2**

Therefore:

**v' = 1**

---

## Step 4 — Apply Quotient Rule

Formula:

**y' = (u'v − uv') / v<sup>2</sup>**

Substitute:

**y' = {2x(3x + 1)<sup>3</sup>(9x + 1)(x + 2) − x<sup>2</sup>(3x + 1)<sup>4</sup>} / (x + 2)<sup>2</sup>**

Factor:

**y' = x(3x + 1)<sup>3</sup>[2(9x + 1)(x + 2) − x(3x + 1)] / (x + 2)<sup>2</sup>**

Now simplify:

**2(9x + 1)(x + 2) = 18x<sup>2</sup> + 38x + 4**

and:

**x(3x + 1) = 3x<sup>2</sup> + x**

Subtract:

**18x<sup>2</sup> + 38x + 4 − 3x<sup>2</sup> − x**

= **15x<sup>2</sup> + 37x + 4**

### Final Answer

**y' = x(3x + 1)<sup>3</sup>(15x<sup>2</sup> + 37x + 4) / (x + 2)<sup>2</sup>**

---

# 6. Integration | ইন্টিগ্রেশন

## 6.1 Power Rule

### Formula

**∫ x<sup>n</sup> dx = x<sup>n+1</sup> / (n + 1) + C**

Condition:

**n ≠ −1**

### Memory

> **Power 1 বাড়াও → নতুন power দিয়ে divide করো**

### Example

**∫ 12x<sup>3</sup> dx**

Increase power:

`3 → 4`

Then:

**12x<sup>4</sup> / 4**

### Answer

**3x<sup>4</sup> + C**

---

## 6.2 Constant Rule

### Formula

**∫ a dx = ax + C**

### Example

**∫ 6 dx = 6x + C**

---

## 6.3 Sum / Difference Rule

Integrate every term separately.

### Example

**∫ (20x<sup>3</sup> + 9x<sup>2</sup> − 14x + 6) dx**

| Term | Integral |
|---|---|
| 20x<sup>3</sup> | 5x<sup>4</sup> |
| 9x<sup>2</sup> | 3x<sup>3</sup> |
| −14x | −7x<sup>2</sup> |
| 6 | 6x |

### Answer

**5x<sup>4</sup> + 3x<sup>3</sup> − 7x<sup>2</sup> + 6x + C**

---

# 7. Why Do We Add +C?

Suppose:

**y = x<sup>2</sup> + 10**

Derivative:

**y' = 2x**

But:

**y = x<sup>2</sup> + 100**

also gives:

**y' = 2x**

Derivative removes constants because:

**d/dx (constant) = 0**

Therefore, if:

**∫ 2x dx**

we cannot know whether the original function contained `+10`, `+100`, `−5`, etc.

So we write:

### **∫ 2x dx = x<sup>2</sup> + C**

`C` = unknown constant.

---

# 8. Common Integrals | সাধারণ ইন্টিগ্রাল

| Function | Integral |
|---|---|
| x<sup>n</sup> | x<sup>n+1</sup> / (n + 1) + C |
| 1/x | ln\|x\| + C |
| e<sup>x</sup> | e<sup>x</sup> + C |
| sin x | −cos x + C |
| cos x | sin x + C |

---

# 9. Complete Integration Example

Evaluate:

### **∫ [12x<sup>3</sup> − 6x<sup>2</sup> + 2x<sup>−1/2</sup> − 5x<sup>−2</sup>] dx**

## First term

**∫ 12x<sup>3</sup> dx**

= **12x<sup>4</sup> / 4**

= **3x<sup>4</sup>**

## Second term

**∫ −6x<sup>2</sup> dx**

= **−6x<sup>3</sup> / 3**

= **−2x<sup>3</sup>**

## Third term

**∫ 2x<sup>−1/2</sup> dx**

Increase power:

**−1/2 + 1 = 1/2**

Then:

**2x<sup>1/2</sup> / (1/2)**

= **4x<sup>1/2</sup>**

= **4√x**

## Fourth term

**∫ −5x<sup>−2</sup> dx**

Increase power:

**−2 + 1 = −1**

Then:

**−5x<sup>−1</sup> / (−1)**

= **5x<sup>−1</sup>**

= **5/x**

### Final Answer

**3x<sup>4</sup> − 2x<sup>3</sup> + 4√x + 5/x + C**

---

# 10. Real-Life Numerical Integration

Real sensor values are often not constant.

Suppose:

| Time | Velocity |
|---:|---:|
| 0 s | 4 m/s |
| 1 s | 6 m/s |
| 2 s | 10 m/s |

Use average velocity for each interval.

## 0 to 1 second

Average velocity:

**(4 + 6) / 2 = 5 m/s**

Distance:

**5 × 1 = 5 m**

## 1 to 2 seconds

Average velocity:

**(6 + 10) / 2 = 8 m/s**

Distance:

**8 × 1 = 8 m**

## Total

**Distance = 5 + 8 = 13 m**

This is **numerical integration**.

---

# 11. Practical Derivative in Embedded Systems

Suppose an encoder gives:

| Time | Position |
|---:|---:|
| 0 ms | 100 count |
| 1 ms | 106 count |

Approximate speed:

**Speed = (New Position − Old Position) / Δt**

So:

**Speed = (106 − 100) / 0.001**

### **Speed = 6000 count/s**

Typical MCU code:

```c
speed = (position_now - position_previous) / dt;
```

---

# 12. Practical Integration in Embedded Systems

Basic implementation:

```c
total += value * dt;
```

Examples:

### Current → Charge

```c
charge += current * dt;
```

### Acceleration → Velocity

```c
velocity += acceleration * dt;
```

### Velocity → Position

```c
position += velocity * dt;
```

---

# 13. PID Control Connection

**PID = Proportional + Integral + Derivative**

A practical form is:

**u(t) = K<sub>p</sub>e(t) + K<sub>i</sub>∫e(t)dt + K<sub>d</sub>[de(t)/dt]**

Where:

- **e(t)** = Target − Actual
- **K<sub>p</sub>** = Proportional gain
- **K<sub>i</sub>** = Integral gain
- **K<sub>d</sub>** = Derivative gain

---

## P — Proportional

Question:

> **How large is the error right now?**  
> **এখন error কত?**

Example:

```text
Target = 18 A
Actual = 16 A
Error  = 2 A
```

P reacts immediately to that `2 A` error.

---

## I — Integral

Question:

> **How much error has accumulated over time?**  
> **error কতক্ষণ ধরে জমছে?**

Example:

```text
Target = 18 A
Actual = 17 A
Error  = 1 A
```

If this `1 A` error continues, the integral term keeps accumulating it and increases correction.

---

## D — Derivative

Question:

> **How fast is the error changing?**  
> **error কত দ্রুত পরিবর্তন হচ্ছে?**

Example:

```text
Target = 18 A

Actual current:
10 A
14 A
17 A
17.8 A
```

The current is rapidly approaching the target.

The derivative term sees this trend and can reduce aggressive correction before overshoot.

---

# 14. E-Clutch PI Current-Control Example

Suppose:

- Battery voltage = **12 V**
- Coil resistance = **0.34 Ω**
- Target current = **18 A**

A useful feed-forward starting duty is:

**Duty<sub>FF</sub> = (I<sub>ref</sub> × R) / V<sub>BAT</sub>**

So:

**Duty<sub>FF</sub> = (18 × 0.34) / 12**

= **0.51**

### Starting duty ≈ **51%**

Then actual current is measured.

If:

```text
Target = 18 A
Actual = 17 A
```

then:

**Error = 18 − 17 = 1 A**

A PI controller can adjust duty using:

**Duty = Duty<sub>FF</sub> + K<sub>p</sub> × Error + K<sub>i</sub> × Accumulated Error**

So:

- Feed-forward gives a good starting duty.
- P corrects present error.
- I removes persistent error.

---

# 15. Final Memory Map

```text
DERIVATIVE
-----------------------------------------
Question: "How fast is it changing?"
বাংলা: "কত দ্রুত পরিবর্তন হচ্ছে?"

Position  -> Velocity
Velocity  -> Acceleration
Charge    -> Current
Energy    -> Power


INTEGRATION
-----------------------------------------
Question: "How much total accumulated?"
বাংলা: "মোট কত জমেছে?"

Velocity  -> Distance
Current   -> Charge
Power     -> Energy
Speed     -> Position
```

---

# 16. Rule Selection Cheat Sheet

```text
See: a x^n
Use: Power Rule

See: f(x) + g(x)
Use: Term-by-term

See: f(x) × g(x)
Use: Product Rule

See: f(x) / g(x)
Use: Quotient Rule

See: function inside another function
Use: Chain Rule
```

---

# 17. Five Things to Remember

1. **Derivative = Rate of change**  
   কোনো কিছু কত দ্রুত পরিবর্তন হচ্ছে।

2. **Integration = Total accumulation**  
   ছোট ছোট অংশ যোগ করে মোট কত হয়েছে।

3. **Derivative Power Rule**  
   Power সামনে আসে, তারপর 1 কমে।

4. **Integration Power Rule**  
   Power 1 বাড়ে, তারপর নতুন power দিয়ে divide হয়।

5. **PID**
   - **P** = Present error
   - **I** = Accumulated error
   - **D** = Rate of error change

---

## Final Engineering Memory

> **Looking for a RATE? → Derivative**  
> **Looking for a TOTAL? → Integration**

> **Formula মুখস্থ করার আগে physical meaning বুঝো।**
