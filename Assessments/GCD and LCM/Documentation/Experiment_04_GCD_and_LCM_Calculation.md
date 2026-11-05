# Experiment 04 – GCD and LCM Calculation

## Aim
To create a VI that finds the Greatest Common Divisor (GCD) and the Least Common Multiple (LCM) of two input numbers using Euclid's algorithm.

---

## Theory

### Greatest Common Divisor (GCD)
The **GCD** (also called HCF) of two positive integers is the **largest integer that divides both of them exactly**.

Example: the divisors of 48 are 1, 2, 3, 4, 6, 8, 12, 16, 24, 48 and the divisors of 18 are 1, 2, 3, 6, 9, 18. The common divisors are 1, 2, 3 and 6, so `GCD(48, 18) = 6`.

### Least Common Multiple (LCM)
The **LCM** of two positive integers is the **smallest positive integer that is a multiple of both**.

Example: multiples of 48 are 48, 96, 144, ... and multiples of 18 are 18, 36, 54, 72, 90, 108, 126, 144, ... The smallest common multiple is 144, so `LCM(48, 18) = 144`.

### Relationship between GCD and LCM
For any two positive integers A and B:

```text
GCD(A, B) × LCM(A, B) = A × B
```

Rearranging gives the formula used in the VI:

```text
LCM(A, B) = (A × B) / GCD(A, B)
```

### Euclid's algorithm
Euclid's algorithm finds the GCD by repeated division. It is based on the property:

```text
GCD(a, b) = GCD(b, a mod b)
```

and the stopping rule:

```text
GCD(a, 0) = a
```

Steps:
1. Divide `a` by `b` and find the remainder `r = a mod b`.
2. Replace `a` with `b` and `b` with `r`.
3. Repeat until the remainder becomes 0.
4. The last non-zero value of `b` (the `b` used in the division that gave remainder 0) is the GCD.

Using the division formula `Dividend = (Divisor × Quotient) + Remainder`, each step looks like:

```text
a = (b × Quotient) + r
```

### Shift registers
Two shift registers carry the working pair `(a, b)` from one iteration to the next. The **upper** register holds `a` (initialized with Input A) and the **lower** register holds `b` (initialized with Input B).

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Initial values | `a = Input A`, `b = Input B` |
| Remainder | `r = a mod b` |
| New a | `a (new) = b (old)` |
| New b | `b (new) = r` |
| Stop condition | `r = 0` |
| GCD | `GCD = b` (value of b when r = 0) |
| Product | `P = A × B` |
| LCM | `LCM = P / GCD = (A × B) / GCD` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number (A) | Numeric Control | Enter the first number |
| Input Number (B) | Numeric Control | Enter the second number |
| GCD | Numeric Indicator | Displays the Greatest Common Divisor |
| LCM | Numeric Indicator | Displays the Least Common Multiple |

---

## Block Diagram Implementation

1. **While Loop:** The Euclidean algorithm is implemented inside a While Loop. The iteration terminal (`i`) is not used in the calculation.
2. **Shift Registers:** Two shift registers hold the working pair of numbers. The upper one is initialized with Input A and the lower one with Input B.
3. **Remainder Calculation:** The *Quotient & Remainder* function divides the upper register value (a) by the lower register value (b) and gives the remainder (`a mod b`).
4. **Updating the Values:**
   - The remainder is wired to the lower shift register, so it becomes the new `b`.
   - The old value of `b` is wired to the upper shift register, so it becomes the new `a`.
5. **Stop Condition:** An *Equal To 0?* function compares the remainder with zero, and its output is wired to the conditional (stop) terminal. The loop stops when the remainder becomes 0.
6. **GCD Output:** When the remainder is 0, the current value of `b` is the GCD. It is passed through the output tunnel on the loop border to the GCD indicator.
7. **LCM Calculation:** Outside the loop, a *Multiply* function computes `A × B`. A *Divide* function divides this product by the GCD: `LCM = (A × B) / GCD`. The result is sent to the LCM indicator.

---

## Worked Examples

### Example 1: A = 48, B = 18

- Initial values: `a = 48`, `b = 18`

| Iteration | a | b | r = a mod b | New a (= b) | New b (= r) | r = 0? |
|---|---|---|---|---|---|---|
| 1 | 48 | 18 | 12 | 18 | 12 | No |
| 2 | 18 | 12 | 6 | 12 | 6 | No |
| 3 | 12 | 6 | 0 | 6 | 0 | **Yes → Stop** |

Division check for each step:
- `48 = (18 × 2) + 12`
- `18 = (12 × 1) + 6`
- `12 = (6 × 2) + 0`

When `r = 0`, `b = 6`, so **GCD = 6**.

LCM calculation:

```text
A × B = 48 × 18 = 864
LCM = 864 / 6 = 144
```

**Result: GCD = 6, LCM = 144**

### Example 2: A = 17, B = 5 (Co-prime numbers)

| Iteration | a | b | r = a mod b | r = 0? |
|---|---|---|---|---|
| 1 | 17 | 5 | 2 | No |
| 2 | 5 | 2 | 1 | No |
| 3 | 2 | 1 | 0 | **Yes → Stop** |

When `r = 0`, `b = 1`, so **GCD = 1**.

```text
LCM = (17 × 5) / 1 = 85
```

**Result: GCD = 1, LCM = 85** (numbers whose GCD is 1 are called co-prime, and their LCM is simply their product)

### Example 3: A = 12, B = 30 (A smaller than B)

| Iteration | a | b | r = a mod b | r = 0? |
|---|---|---|---|---|
| 1 | 12 | 30 | 12 | No |
| 2 | 30 | 12 | 6 | No |
| 3 | 12 | 6 | 0 | **Yes → Stop** |

In iteration 1, `12 mod 30 = 12`, so the values effectively swap places for the next iteration. The algorithm therefore works whichever number is larger.

When `r = 0`, `b = 6`, so **GCD = 6**.

```text
LCM = (12 × 30) / 6 = 360 / 6 = 60
```

**Result: GCD = 6, LCM = 60**

### Example 4: A = 7, B = 7 (Equal numbers)

| Iteration | a | b | r = a mod b | r = 0? |
|---|---|---|---|---|
| 1 | 7 | 7 | 0 | **Yes → Stop** |

When `r = 0`, `b = 7`, so **GCD = 7**.

```text
LCM = (7 × 7) / 7 = 7
```

**Result: GCD = 7, LCM = 7**

---

## Test Cases and Results

| Input A | Input B | Expected GCD | Expected LCM |
|---|---|---|---|
| 48 | 18 | 6 | 144 |
| 17 | 5 | 1 | 85 |
| 12 | 30 | 6 | 60 |
| 7 | 7 | 7 | 7 |

---

## Note
- The inputs should be positive integers. If Input B is 0, the first division `a mod b` has no valid result.
- The product `A × B` can become very large. If the numeric data type is I32, very large inputs can overflow it, so a larger or floating-point type may be needed.

---

## Learning Outcomes
- Understood the concepts of GCD and LCM and the relationship `GCD × LCM = A × B`.
- Understood and implemented Euclid's algorithm using repeated division.
- Used the Quotient & Remainder function to find remainders.
- Used two shift registers to carry a working pair of values between iterations.
- Used a While Loop with a conditional stop based on the remainder becoming zero.
- Used Multiply and Divide functions outside the loop to calculate the LCM.

---

## Tools Used
- LabVIEW
- While Loop
- Shift Registers
- Quotient & Remainder function
- Equal To 0? function
- Multiply function
- Divide function
- Numeric Controls and Numeric Indicators

---

## Conclusion
Successfully developed a LabVIEW VI that finds the GCD of two numbers using Euclid's algorithm inside a While Loop with shift registers, and calculates the LCM using the relation `LCM = (A × B) / GCD`.
