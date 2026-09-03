# Calculus for PID & Control Engineering

**Derivative + Integration + PID Control**  
**ডেরিভেটিভ + ইন্টিগ্রেশন + PID কন্ট্রোল**

A compact, practical guide for understanding calculus in control engineering without memorizing formulas blindly.  
Control engineering-এ calculus কেন লাগে, কোন rule কখন ব্যবহার করতে হয়, এবং PID-এর সাথে এর সম্পর্ক — সহজভাবে দেখানো হয়েছে।

---

## 1. Core Idea | মূল ধারণা

### Derivative

**Derivative = how fast something is changing.**  
**Derivative = কোনো কিছু কত দ্রুত পরিবর্তন হচ্ছে।**

Examples:

```text
Position  -> derivative -> Velocity
Velocity  -> derivative -> Acceleration
Charge    -> derivative -> Current
Energy    -> derivative -> Power
```

Simple idea:

```text
Derivative ≈ Change / Time
```

Example:

```text
Position changes from 4 m to 10 m in 2 s

Average velocity = (10 - 4) / 2
                 = 3 m/s
```

---

### Integration

**Integration = total accumulation.**  
**Integration = ছোট ছোট অংশ যোগ করে মোট কত হলো।**

Examples:

```text
Velocity      -> integration -> Distance
Current       -> integration -> Charge
Power         -> integration -> Energy
Angular speed -> integration -> Angle
```

Simple idea:

```text
Integration ≈ Rate × small time + Rate × small time + ...
```

Example:

```text
Current = 2 A
Time    = 5 s

Charge = 2 × 5
       = 10 C
```

---

# 2. Derivative Rules | ডেরিভেটিভের নিয়ম

## Rule 1 — Constant Rule

Use when the term has no `x`.

```text
d/dx (C) = 0
```

Example:

```text
d/dx (10) = 0
```

Why?

A constant does not change when `x` changes.

---

## Rule 2 — Power Rule

Use for terms like:

```text
a x^n
```

Formula:

```text
d/dx (x^n) = n x^(n-1)
```

### Memory

```text
Power সামনে আনো
Power থেকে 1 কমাও
```

Example:

```text
d/dx (10x^3)

= 10 × 3x^(3-1)
= 30x^2
```

Answer:

```text
30x^2
```

---

## Rule 3 — Sum / Difference Rule

Use when several terms are added or subtracted.

Example:

```text
y = 5x^4 + 3x^3 - 7x^2 + 6x + 10
```

Differentiate term by term:

```text
5x^4   -> 20x^3
3x^3   ->  9x^2
-7x^2  -> -14x
6x     ->  6
10     ->  0
```

Therefore:

```text
dy/dx = 20x^3 + 9x^2 - 14x + 6
```

---

## Rule 4 — Product Rule

Use when **two functions are multiplied**.

Form:

```text
y = u × v
```

Formula:

```text
dy/dx = u'v + uv'
```

### Memory

```text
First derivative × Second
+
First × Second derivative
```

Example:

```text
y = (2x^2 + 3)(x^3 - 4)
```

Let:

```text
u = 2x^2 + 3
v = x^3 - 4
```

Then:

```text
u' = 4x
v' = 3x^2
```

Apply Product Rule:

```text
y' = (4x)(x^3 - 4) + (2x^2 + 3)(3x^2)
```

Expand:

```text
= 4x^4 - 16x + 6x^4 + 9x^2
```

Final:

```text
y' = 10x^4 + 9x^2 - 16x
```

---

## Rule 5 — Quotient Rule

Use when **one function is divided by another**.

Form:

```text
y = u / v
```

Formula:

```text
dy/dx = (u'v - uv') / v^2
```

### Memory

```text
Low × D-High
-
High × D-Low
----------------
Low^2
```

Example:

```text
y = (x^2 + 3) / (x + 1)
```

Let:

```text
u = x^2 + 3
v = x + 1
```

Then:

```text
u' = 2x
v' = 1
```

Apply Quotient Rule:

```text
y' = [(2x)(x+1) - (x^2+3)(1)] / (x+1)^2
```

Simplify:

```text
= [2x^2 + 2x - x^2 - 3] / (x+1)^2
```

Final:

```text
y' = (x^2 + 2x - 3) / (x+1)^2
```

---

## Rule 6 — Chain Rule

Use when a function is **inside another function**.

Example shape:

```text
(inside)^power
sin(inside)
cos(inside)
e^(inside)
```

Formula idea:

```text
Derivative = Outside derivative × Inside derivative
```

Example:

```text
y = (3x^2 + 2)^5
```

Step 1 — Differentiate outside:

```text
5(3x^2 + 2)^4
```

Step 2 — Differentiate inside:

```text
d/dx (3x^2 + 2) = 6x
```

Multiply:

```text
y' = 5(3x^2 + 2)^4 × 6x
```

Final:

```text
y' = 30x(3x^2 + 2)^4
```

---

# 3. Common Derivatives | সাধারণ ডেরিভেটিভ

```text
d/dx (C)       = 0
d/dx (x^n)     = n x^(n-1)

d/dx (sin x)   = cos x
d/dx (cos x)   = -sin x
d/dx (tan x)   = sec^2 x

d/dx (e^x)     = e^x
d/dx (ln x)    = 1/x
```

Example with Chain Rule:

```text
y = sin(x^2)
```

Outside derivative:

```text
cos(x^2)
```

Inside derivative:

```text
2x
```

Final:

```text
y' = 2x cos(x^2)
```

---

# 4. Which Derivative Rule Should I Use?

```text
Expression type                    Rule

ax^n                               Power Rule

f(x) + g(x)                        Term-by-term

f(x) × g(x)                        Product Rule

f(x) / g(x)                        Quotient Rule

f(g(x))                            Chain Rule
```

Important:

A single problem can need **more than one rule**.

---

# 5. Complete Mixed Derivative Example

Find the derivative of:

```text
y = [x^2 (3x + 1)^4] / (x + 2)
```

This problem uses:

```text
Power Rule
Product Rule
Chain Rule
Quotient Rule
```

---

## Step 1 — Split numerator and denominator

```text
u = x^2 (3x + 1)^4
v = x + 2
```

So:

```text
y = u / v
```

---

## Step 2 — Differentiate `u`

```text
u = x^2 (3x + 1)^4
```

This is multiplication, so use Product Rule.

Let:

```text
a = x^2
b = (3x + 1)^4
```

Then:

```text
a' = 2x
```

For `b`, use Chain Rule:

```text
b' = 4(3x + 1)^3 × 3
   = 12(3x + 1)^3
```

Now Product Rule:

```text
u' = a'b + ab'
```

Therefore:

```text
u' = 2x(3x + 1)^4
   + x^2 × 12(3x + 1)^3
```

Factor common terms:

```text
u' = 2x(3x + 1)^3[(3x + 1) + 6x]
```

So:

```text
u' = 2x(3x + 1)^3(9x + 1)
```

---

## Step 3 — Differentiate `v`

```text
v = x + 2
```

Therefore:

```text
v' = 1
```

---

## Step 4 — Apply Quotient Rule

```text
y' = (u'v - uv') / v^2
```

Substitute:

```text
y' =
{
2x(3x + 1)^3(9x + 1)(x + 2)
-
x^2(3x + 1)^4
}
/
(x + 2)^2
```

Factor:

```text
y' =
x(3x + 1)^3
[
2(9x + 1)(x + 2)
-
x(3x + 1)
]
/
(x + 2)^2
```

Simplify inside bracket:

```text
2(9x + 1)(x + 2)
= 18x^2 + 38x + 4
```

and:

```text
x(3x + 1)
= 3x^2 + x
```

Subtract:

```text
18x^2 + 38x + 4
-
3x^2 - x
=
15x^2 + 37x + 4
```

Final answer:

```text
y' =
x(3x + 1)^3(15x^2 + 37x + 4)
/
(x + 2)^2
```

---

# 6. Integration Rules | ইন্টিগ্রেশনের নিয়ম

## Rule 1 — Power Rule

Use for:

```text
x^n
```

Formula:

```text
∫ x^n dx = x^(n+1)/(n+1) + C
```

Condition:

```text
n ≠ -1
```

### Memory

```text
Power 1 বাড়াও
তারপর নতুন power দিয়ে divide করো
```

Example:

```text
∫ 12x^3 dx

Power: 3 -> 4

= 12x^4 / 4
= 3x^4 + C
```

---

## Rule 2 — Constant Rule

```text
∫ a dx = ax + C
```

Example:

```text
∫ 6 dx = 6x + C
```

---

## Rule 3 — Sum / Difference Rule

Integrate each term separately.

Example:

```text
∫ (20x^3 + 9x^2 - 14x + 6) dx
```

Term by term:

```text
20x^3  -> 5x^4
9x^2   -> 3x^3
-14x   -> -7x^2
6      -> 6x
```

Therefore:

```text
= 5x^4 + 3x^3 - 7x^2 + 6x + C
```

---

# 7. Why Do We Add `+ C`?

Suppose:

```text
y = x^2 + 10
```

Derivative:

```text
y' = 2x
```

Also:

```text
y = x^2 + 100
```

Derivative is still:

```text
y' = 2x
```

So derivative loses the original constant.

That is why:

```text
∫ 2x dx = x^2 + C
```

`C` means:

```text
unknown constant
```

To find the exact value of `C`, we need an initial condition.

Example:

```text
y(0) = 10
```

If:

```text
y = x^2 + C
```

Then:

```text
10 = 0^2 + C
```

So:

```text
C = 10
```

---

# 8. Special Integrals

```text
∫ x^n dx     = x^(n+1)/(n+1) + C     , n ≠ -1

∫ 1/x dx     = ln|x| + C

∫ e^x dx     = e^x + C

∫ sin x dx   = -cos x + C

∫ cos x dx   = sin x + C
```

---

# 9. Complete Integration Example

Evaluate:

```text
∫ [12x^3 - 6x^2 + 2x^(-1/2) - 5x^(-2)] dx
```

### First term

```text
∫ 12x^3 dx
= 12x^4 / 4
= 3x^4
```

### Second term

```text
∫ -6x^2 dx
= -6x^3 / 3
= -2x^3
```

### Third term

```text
∫ 2x^(-1/2) dx
```

Increase power:

```text
-1/2 + 1 = 1/2
```

Then:

```text
= 2x^(1/2) / (1/2)
= 4x^(1/2)
= 4√x
```

### Fourth term

```text
∫ -5x^(-2) dx
```

Increase power:

```text
-2 + 1 = -1
```

Then:

```text
= -5x^(-1) / (-1)
= 5x^(-1)
= 5/x
```

Final:

```text
3x^4 - 2x^3 + 4√x + 5/x + C
```

---

# 10. Numerical Integration | বাস্তব সেন্সর ডাটা

In real life, values are often not constant.

Suppose measured velocity is:

```text
Time    Velocity

0 s     4 m/s
1 s     6 m/s
2 s    10 m/s
```

Use the trapezoidal method.

From 0 to 1 s:

```text
Average velocity = (4 + 6) / 2
                 = 5 m/s

Distance = 5 × 1
         = 5 m
```

From 1 to 2 s:

```text
Average velocity = (6 + 10) / 2
                 = 8 m/s

Distance = 8 × 1
         = 8 m
```

Total:

```text
Distance = 5 + 8
         = 13 m
```

This is practical numerical integration.

---

# 11. Practical Derivative in Embedded Systems

Suppose an encoder gives:

```text
Time      Position

0 ms      100 count
1 ms      106 count
```

Approximate speed:

```text
speed = (new position - old position) / dt
```

So:

```text
speed = (106 - 100) / 0.001
      = 6000 count/s
```

Typical embedded implementation:

```c
speed = (position_now - position_previous) / dt;
```

---

# 12. Practical Integration in Embedded Systems

For integration:

```c
total += value * dt;
```

Examples:

Current -> Charge:

```c
charge += current * dt;
```

Acceleration -> Velocity:

```c
velocity += acceleration * dt;
```

Velocity -> Position:

```c
position += velocity * dt;
```

---

# 13. Engineering Meaning

```text
Quantity           Derivative gives        Integration gives

Position           Velocity                -
Velocity           Acceleration            Position
Charge             Current                 -
Current            -                       Charge
Energy             Power                   -
Power              -                       Energy
Motor angle        Angular speed           -
Angular speed      Angular acceleration    Angle
Temperature        Rate of temperature     Temperature change
```

---

# 14. PID Control Connection

PID means:

```text
P = Proportional
I = Integral
D = Derivative
```

Basic PID equation:

```text
u(t) =
Kp × e(t)
+
Ki × integral of error
+
Kd × derivative of error
```

More explicitly:

```text
u(t) = Kp e(t) + Ki ∫e(t)dt + Kd de(t)/dt
```

Where:

```text
e(t) = target - actual
```

---

## P Term

Question:

```text
How big is the error NOW?
```

Bangla:

```text
এখন error কত?
```

Example:

```text
Target = 18 A
Actual = 16 A

Error = 2 A
```

P reacts immediately to this `2 A` error.

---

## I Term

Question:

```text
How much error has accumulated over time?
```

Bangla:

```text
error কতক্ষণ ধরে জমছে?
```

If:

```text
Target = 18 A
Actual = 17 A
```

then:

```text
Error = 1 A
```

If this error stays for a long time, the integral term becomes larger and keeps correcting it.

---

## D Term

Question:

```text
How fast is the error changing?
```

Bangla:

```text
error কত দ্রুত পরিবর্তন হচ্ছে?
```

Example:

```text
Target = 18 A

Actual:
10 A
14 A
17 A
17.8 A
```

The current is approaching the target very fast.

D sees this trend and can reduce aggressive correction before overshoot.

---

# 15. E-Clutch PI Current Control Example

Suppose:

```text
Target current = 18 A
Actual current = 17 A
```

Then:

```text
error = 18 - 17
      = 1 A
```

A PI controller may calculate:

```text
duty =
feedforward duty
+
Kp × error
+
Ki × accumulated error
```

For a 12 V supply and 0.34 ohm coil:

```text
Feedforward duty ≈ (Iref × R) / Vbat
```

For 18 A:

```text
Duty ≈ (18 × 0.34) / 12
     ≈ 0.51
     ≈ 51%
```

Then the PI controller slightly increases or decreases duty based on measured current.

---

# 16. Final Memory Map

```text
DERIVATIVE
--------------------------------
"How fast is it changing?"
"কত দ্রুত পরিবর্তন হচ্ছে?"

Total -> Rate

Position -> Velocity
Velocity -> Acceleration
Charge   -> Current
Energy   -> Power


INTEGRATION
--------------------------------
"How much total accumulated?"
"মোট কত জমেছে?"

Rate -> Total

Velocity -> Distance
Current  -> Charge
Power    -> Energy
Speed    -> Position
```

---

# 17. Final Rule Selection

```text
See ax^n
-> Power Rule

See +
-> Do each term separately

See function × function
-> Product Rule

See function / function
-> Quotient Rule

See function inside function
-> Chain Rule
```

---

# 18. Five Things to Remember

1. **Derivative = rate of change**  
   **Derivative = কত দ্রুত পরিবর্তন হচ্ছে**

2. **Integration = total accumulation**  
   **Integration = মোট কত জমেছে**

3. **Derivative Power Rule**  
   Power comes down, then power decreases by 1.

4. **Integration Power Rule**  
   Power increases by 1, then divide by the new power.

5. **PID**  
   `P = present error`  
   `I = accumulated error`  
   `D = rate of error change`

---

## One-Line Engineering Memory

```text
Derivative = Change / Time
Integration = Sum of (Value × Small Time)
PID = Present + Accumulated Past + Rate of Change
```

---

**Goal:** Do not memorize calculus only as formulas.  
Always ask:

```text
Am I looking for a RATE?
-> Derivative

Am I looking for a TOTAL?
-> Integration
```
