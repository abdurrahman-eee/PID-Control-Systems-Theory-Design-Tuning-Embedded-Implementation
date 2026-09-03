# PID Control Systems — Theory, Design, Tuning & Embedded Implementation

A structured, from-the-roots learning path for PID control: theory → math → tuning → real embedded code (STM32).

```
pid-control-systems/
│
├── 01-Fundamentals/
├── 02-Derivative-and-Integral/     ← PID Cheat Sheet lives here
├── 03-P-PI-PID-Control/
├── 04-System-Modeling/
├── 05-PID-Tuning/
├── 06-Current-Control/
├── 07-Speed-Control/
├── 08-Position-Control/
├── 09-Simulations/
├── 10-Embedded-Implementation/
├── 11-STM32-Examples/
├── 12-Practical-Projects/
└── README.md
```

Start with [`02-Derivative-and-Integral/PID-Cheat-Sheet.md`](02-Derivative-and-Integral/PID-Cheat-Sheet.md) for a quick, worked-example explanation of P, I, D and the full PID formula.



<div align="center">

# 📘 Calculus for Control Engineers
### Derivative • Integration • PID Control  
### ডেরিভেটিভ • ইন্টিগ্রেশন • PID কন্ট্রোল

**A practical, engineering-focused guide with formulas, intuition, examples, and PID applications.**  
**সহজ ব্যাখ্যা, সূত্র, উদাহরণ এবং PID Control-এর ব্যবহারসহ একটি practical guide।**

</div>

---

## 📌 Table of Contents

1. [Core Idea](#-core-idea)
2. [Derivative Rules](#-derivative-rules)
3. [Common Derivatives](#-common-derivatives)
4. [Complete Mixed Derivative Example](#-complete-mixed-derivative-example)
5. [Integration Rules](#-integration-rules)
6. [Complete Integration Example](#-complete-integration-example)
7. [Numerical / Practical Integration](#-numerical--practical-integration)
8. [Engineering Meaning](#-engineering-meaning)
9. [PID Connection](#-pid-connection)
10. [Final Memory Map](#-final-memory-map)

---

# 🧠 Core Idea

## Derivative

> **Derivative = How fast something is changing.**  
> **ডেরিভেটিভ = কোনো কিছু কত দ্রুত পরিবর্তন হচ্ছে।**

Examples:

$$
\text{Position} \xrightarrow{\frac{d}{dt}} \text{Velocity}
$$

$$
\text{Velocity} \xrightarrow{\frac{d}{dt}} \text{Acceleration}
$$

$$
\text{Charge} \xrightarrow{\frac{d}{dt}} \text{Current}
$$

$$
\text{Energy} \xrightarrow{\frac{d}{dt}} \text{Power}
$$

### Practical idea

If position changes from $4\,m$ to $9\,m$ in $2\,s$:

$$
v=\frac{\Delta x}{\Delta t}
=\frac{9-4}{2}
=2.5\,m/s
$$

So derivative is basically:

> **Change ÷ Time**

---

## Integration

> **Integration = Total accumulation.**  
> **ইন্টিগ্রেশন = ছোট ছোট পরিবর্তন যোগ করে মোট কত হলো।**

Examples:

$$
\text{Velocity} \xrightarrow{\int dt} \text{Distance}
$$

$$
\text{Current} \xrightarrow{\int dt} \text{Charge}
$$

$$
\text{Power} \xrightarrow{\int dt} \text{Energy}
$$

If current is $2\,A$ for $5\,s$:

$$
Q=\int I\,dt
$$

For constant current:

$$
Q=2\times5=10\,C
$$

So integration is basically:

> **Rate × Small Time + Rate × Small Time + ...**

---

# 📐 Derivative Rules

## 1. Constant Rule

If:

$$
y=C
$$

then:

$$
\frac{dy}{dx}=0
$$

### Example

$$
\frac{d}{dx}(10)=0
$$

**Use when:** the term contains no $x$.

---

## 2. Power Rule

$$
\boxed{\frac{d}{dx}(x^n)=nx^{n-1}}
$$

### Memory

> **Power সামনে আনো → Power থেকে 1 কমাও**

### Example

$$
\frac{d}{dx}(10x^3)
$$

$$
=10(3)x^{3-1}
$$

$$
\boxed{=30x^2}
$$

**Use when:** the term looks like $ax^n$.

---

## 3. Sum / Difference Rule

Differentiate each term separately.

$$
\frac{d}{dx}[f(x)+g(x)]
=
f'(x)+g'(x)
$$

### Example

$$
y=5x^4+3x^3-7x^2+6x+10
$$

Then:

$$
\boxed{
\frac{dy}{dx}
=
20x^3+9x^2-14x+6
}
$$

---

## 4. Product Rule

Use when **two functions are multiplied**.

If:

$$
y=u\,v
$$

then:

$$
\boxed{
\frac{dy}{dx}=u'v+uv'
}
$$

### Memory

> **First derivative × Second + First × Second derivative**

### Example

$$
y=(2x^2+3)(x^3-4)
$$

Let:

$$
u=2x^2+3
\qquad
v=x^3-4
$$

Then:

$$
u'=4x
\qquad
v'=3x^2
$$

So:

$$
y'=(4x)(x^3-4)+(2x^2+3)(3x^2)
$$

$$
\boxed{
y'=10x^4+9x^2-16x
}
$$

**Use when:**  
$$
(\text{function})\times(\text{function})
$$

---

## 5. Quotient Rule

Use when **one function is divided by another**.

If:

$$
y=\frac{u}{v}
$$

then:

$$
\boxed{
\frac{dy}{dx}
=
\frac{u'v-uv'}{v^2}
}
$$

### Memory

> **Low × D-High − High × D-Low, over Low²**

### Example

$$
y=\frac{x^2+3}{x+1}
$$

Let:

$$
u=x^2+3
\qquad
v=x+1
$$

Then:

$$
u'=2x
\qquad
v'=1
$$

Therefore:

$$
y'
=
\frac{(2x)(x+1)-(x^2+3)(1)}
{(x+1)^2}
$$

$$
\boxed{
y'=
\frac{x^2+2x-3}{(x+1)^2}
}
$$

---

## 6. Chain Rule

Use when there is a **function inside another function**.

If:

$$
y=f(g(x))
$$

then:

$$
\boxed{
\frac{dy}{dx}
=
f'(g(x))\,g'(x)
}
$$

### Memory

> **Outside derivative × Inside derivative**  
> **বাইরের derivative × ভিতরের derivative**

### Example

$$
y=(3x^2+2)^5
$$

Outer derivative:

$$
5(3x^2+2)^4
$$

Inner derivative:

$$
6x
$$

Therefore:

$$
\boxed{
y'=30x(3x^2+2)^4
}
$$

---

# 🧾 Common Derivatives

| Function | Derivative |
|---|---|
| $C$ | $0$ |
| $x^n$ | $nx^{n-1}$ |
| $\sin x$ | $\cos x$ |
| $\cos x$ | $-\sin x$ |
| $\tan x$ | $\sec^2 x$ |
| $e^x$ | $e^x$ |
| $\ln x$ | $\frac{1}{x}$ |

### Example: Trigonometric + Chain Rule

$$
y=\sin(x^2)
$$

Outer derivative:

$$
\cos(x^2)
$$

Inner derivative:

$$
2x
$$

Therefore:

$$
\boxed{
y'=2x\cos(x^2)
}
$$

---

# 🔥 Complete Mixed Derivative Example

Consider:

$$
\boxed{
y=
\frac{x^2(3x+1)^4}{x+2}
}
$$

This contains:

- Power Rule
- Product Rule
- Chain Rule
- Quotient Rule

---

## Step 1 — Define numerator and denominator

$$
u=x^2(3x+1)^4
$$

$$
v=x+2
$$

Then:

$$
y=\frac{u}{v}
$$

---

## Step 2 — Differentiate numerator using Product Rule

For:

$$
u=x^2(3x+1)^4
$$

Let:

$$
a=x^2
$$

$$
b=(3x+1)^4
$$

Then:

$$
a'=2x
$$

For $b$, use Chain Rule:

$$
b'
=
4(3x+1)^3(3)
$$

$$
b'=12(3x+1)^3
$$

Now Product Rule:

$$
u'
=
a'b+ab'
$$

$$
u'
=
2x(3x+1)^4
+
12x^2(3x+1)^3
$$

Factor:

$$
\boxed{
u'
=
2x(3x+1)^3(9x+1)
}
$$

---

## Step 3 — Differentiate denominator

$$
v=x+2
$$

Therefore:

$$
v'=1
$$

---

## Step 4 — Apply Quotient Rule

$$
y'
=
\frac{u'v-uv'}{v^2}
$$

So:

$$
y'
=
\frac{
2x(3x+1)^3(9x+1)(x+2)
-
x^2(3x+1)^4
}{
(x+2)^2
}
$$

Factor common terms:

$$
y'
=
\frac{
x(3x+1)^3
\left[
2(9x+1)(x+2)-x(3x+1)
\right]
}{
(x+2)^2
}
$$

Simplify the bracket:

$$
2(9x+1)(x+2)-x(3x+1)
=
15x^2+37x+4
$$

Therefore:

$$
\boxed{
\frac{dy}{dx}
=
\frac{
x(3x+1)^3(15x^2+37x+4)
}{
(x+2)^2
}
}
$$

---

# ∫ Integration Rules

## 1. Power Rule

For $n\neq-1$:

$$
\boxed{
\int x^n\,dx
=
\frac{x^{n+1}}{n+1}+C
}
$$

### Memory

> **Power 1 বাড়াও → নতুন power দিয়ে divide করো**

### Example

$$
\int 12x^3\,dx
$$

$$
=
12\frac{x^4}{4}
$$

$$
\boxed{
=3x^4+C
}
$$

---

## 2. Constant Rule

$$
\boxed{
\int a\,dx=ax+C
}
$$

### Example

$$
\int 6\,dx
=
\boxed{6x+C}
$$

---

## 3. Sum / Difference Rule

Integrate each term separately.

### Example

$$
\int
(20x^3+9x^2-14x+6)\,dx
$$

Term by term:

$$
20x^3
\rightarrow
5x^4
$$

$$
9x^2
\rightarrow
3x^3
$$

$$
-14x
\rightarrow
-7x^2
$$

$$
6
\rightarrow
6x
$$

Therefore:

$$
\boxed{
5x^4+3x^3-7x^2+6x+C
}
$$

---

## Why $+C$?

Because:

$$
\frac{d}{dx}(10)=0
$$

and:

$$
\frac{d}{dx}(100)=0
$$

Therefore derivative loses the original constant.

So after integration we write:

$$
\boxed{+C}
$$

To recover the exact constant, we need an **initial condition**.

Example:

$$
y(0)=10
$$

Then:

$$
C=10
$$

---

## 4. Special Integrals

| Function | Integral |
|---|---|
| $x^n$ | $\frac{x^{n+1}}{n+1}+C$ |
| $\frac{1}{x}$ | $\ln|x|+C$ |
| $e^x$ | $e^x+C$ |
| $\sin x$ | $-\cos x+C$ |
| $\cos x$ | $\sin x+C$ |

---

# ✅ Complete Integration Example

Evaluate:

$$
\int
\left(
12x^3
-
6x^2
+
2x^{-1/2}
-
5x^{-2}
\right)
dx
$$

### First term

$$
\int12x^3dx
=
3x^4
$$

### Second term

$$
\int-6x^2dx
=
-2x^3
$$

### Third term

$$
\int2x^{-1/2}dx
$$

Increase power:

$$
-\frac12+1=\frac12
$$

Therefore:

$$
=
2\frac{x^{1/2}}{1/2}
=
4x^{1/2}
$$

$$
=4\sqrt{x}
$$

### Fourth term

$$
\int-5x^{-2}dx
$$

$$
=
-5\frac{x^{-1}}{-1}
$$

$$
=
5x^{-1}
=
\frac{5}{x}
$$

Final answer:

$$
\boxed{
3x^4-2x^3+4\sqrt{x}+\frac5x+C
}
$$

---

# 📊 Numerical / Practical Integration

Real sensor data is usually **not constant**.

Suppose velocity is measured:

| Time | Velocity |
|---:|---:|
| $0s$ | $4\,m/s$ |
| $1s$ | $6\,m/s$ |
| $2s$ | $10\,m/s$ |

Using the trapezoidal method:

### From 0 to 1 s

$$
d_1
=
\frac{4+6}{2}(1)
=
5\,m
$$

### From 1 to 2 s

$$
d_2
=
\frac{6+10}{2}(1)
=
8\,m
$$

Total:

$$
\boxed{
d=5+8=13\,m
}
$$

This is **numerical integration**.

---

# ⚙️ Engineering Meaning

| Quantity | Derivative gives | Integration gives |
|---|---|---|
| Position | Velocity | — |
| Velocity | Acceleration | Position |
| Charge | Current | — |
| Current | — | Charge |
| Energy | Power | — |
| Power | — | Energy |
| Motor angle | Angular speed | — |
| Angular speed | Angular acceleration | Angle |
| Temperature | Heating/cooling rate | Temperature change |

---

## Embedded Derivative

An MCU can estimate derivative using:

```c
rate = (value_now - value_previous) / dt;
```

Example:

```text
Position at 0 ms = 100 count
Position at 1 ms = 106 count
```

Then:

$$
\text{Speed}
\approx
\frac{106-100}{0.001}
=
6000\;\text{count/s}
$$

---

## Embedded Integration

An MCU can estimate integration using:

```c
total += value * dt;
```

Example for current:

```c
charge += current * dt;
```

Example for acceleration:

```c
velocity += acceleration * dt;
```

---

# 🎛️ PID Connection

PID means:

$$
\boxed{
\text{PID}
=
\text{Proportional}
+
\text{Integral}
+
\text{Derivative}
}
$$

The continuous PID controller is:

$$
u(t)
=
K_p e(t)
+
K_i\int_0^t e(\tau)\,d\tau
+
K_d\frac{de(t)}{dt}
$$

where:

- $e(t)$ = error
- $K_p$ = proportional gain
- $K_i$ = integral gain
- $K_d$ = derivative gain

---

## Easy PID Meaning

| Term | Question | বাংলা |
|---|---|---|
| **P** | How big is the error **now**? | এখন error কত? |
| **I** | How much error has **accumulated**? | error কতক্ষণ ধরে জমেছে? |
| **D** | How fast is the error **changing**? | error কত দ্রুত পরিবর্তন হচ্ছে? |

### Memory

> **P = Present**  
> **I = Past accumulation**  
> **D = Rate / trend**

---

## Example — E-Clutch Current Control

Target:

$$
I_{ref}=18A
$$

Actual:

$$
I_{actual}=17A
$$

Error:

$$
e=18-17=1A
$$

### P term

Responds immediately to the current $1A$ error.

### I term

If the $1A$ error continues, it accumulates:

$$
\int e(t)\,dt
$$

and gradually increases correction.

### D term

Observes whether the current is approaching $18A$ quickly or slowly:

$$
\frac{de}{dt}
$$

The controller output can then adjust PWM duty.

---

# 🗺️ Final Memory Map

```text
DERIVATIVE
────────────────────────
"How fast is it changing?"
"কত দ্রুত পরিবর্তন হচ্ছে?"

Total quantity → Rate

Position → Velocity
Velocity → Acceleration
Charge   → Current
Energy   → Power
Angle    → Angular Speed


INTEGRATION
────────────────────────
"How much total accumulated?"
"মোট কত জমেছে?"

Rate → Total quantity

Velocity → Distance
Current  → Charge
Power    → Energy
Angular Speed → Angle
```

---

# 🧭 Which Rule Should I Use?

```text
Expression
   │
   ├── axⁿ
   │     └── Power Rule
   │
   ├── f(x) + g(x)
   │     └── Differentiate term-by-term
   │
   ├── f(x) × g(x)
   │     └── Product Rule
   │
   ├── f(x) / g(x)
   │     └── Quotient Rule
   │
   └── f(g(x))
         └── Chain Rule
```

---

## ⭐ Final 5 Things to Remember

1. **Derivative = rate of change**
2. **Integration = total accumulation**
3. **Power Rule:** power comes down for derivative
4. **Integration Power Rule:** power goes up by 1, then divide
5. **PID:** P = present error, I = accumulated error, D = rate of error change

---

<div align="center">

### Control Engineering becomes easier when the mathematics has physical meaning.

**Don't memorize only the formula — understand what the formula is measuring.**

</div>

