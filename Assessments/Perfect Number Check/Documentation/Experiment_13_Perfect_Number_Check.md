# Experiment 13 – Perfect Number Check

## Aim
To create a VI that checks whether an entered positive integer is a Perfect Number by calculating the sum of its proper divisors and comparing it with the original number.

---

## Theory

### Proper divisors
A **proper divisor** of a positive integer `N` is a positive integer that divides `N` exactly, **excluding `N` itself**.

Example: the proper divisors of 12 are 1, 2, 3, 4 and 6 (12 is excluded).

### Perfect number
A **perfect number** is a positive integer that is equal to the **sum of its proper divisors**.

```text
N is Perfect  if  (sum of proper divisors of N) = N
```

Example: the proper divisors of 6 are 1, 2 and 3, and `1 + 2 + 3 = 6`, so 6 is a perfect number.

### Classification of numbers
Comparing the sum of proper divisors with the number gives three types:

| Condition | Type | Example |
|---|---|---|
| Sum = N | Perfect | 6 (sum = 6) |
| Sum > N | Abundant | 12 (sum = 16) |
| Sum < N | Deficient | 8 (sum = 7) |

### Examples of perfect numbers
The first few perfect numbers are **6, 28, 496 and 8128**.

| Number | Proper divisors | Sum |
|---|---|---|
| 6 | 1, 2, 3 | 6 |
| 28 | 1, 2, 4, 7, 14 | 28 |

### Divisibility and the division formula
A number `d` is a divisor of `N` when the division leaves **remainder 0**. Using the division formula:

```text
N = (d × Quotient) + Remainder
```

- If `Remainder = 0`, then `d` divides `N` exactly and is a divisor.
- If `Remainder ≠ 0`, then `d` is not a divisor.

### Shift register as an accumulator
A **shift register** carries a value from one loop iteration to the next. Here it stores the running sum of the divisors found so far, initialized to 0.

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Loop count | `N − 1` |
| Iteration index | `i = 0, 1, 2, ..., N − 2` |
| Divisor | `d = i + 1` (ranges from 1 to N − 1) |
| Remainder | `R = N mod d` |
| Divisor test | `R = 0` (Equal?) |
| Sum update | `Sum (new) = Sum (previous) + d` if `R = 0`, else `Sum (new) = Sum (previous)` |
| Sum initial value | `0` |
| Decision | `Perfect if Sum = N` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number | Numeric Control | Enter the positive integer |
| Sum of Proper Divisors | Numeric Indicator | Displays the sum of the proper divisors |
| Result | String Indicator | Displays "Perfect Number" or "Not a Perfect Number" |

---

## Block Diagram Implementation

1. A **For Loop** is used to iterate from 1 to `N − 1`, where `N` is the input number.
2. The **iteration terminal** is incremented by 1 to generate the divisor values.
3. The *Quotient & Remainder* function checks whether the input number is exactly divisible by the current divisor.
4. An *Equal?* function checks whether the remainder is zero.
5. A **Case Structure** is used to identify proper divisors.
   - **TRUE (remainder is zero):** The current divisor is added to the previous sum.
   - **FALSE:** The previous sum is passed unchanged.
6. A **Shift Register** is used to maintain and accumulate the sum of divisors across loop iterations, initialized to **zero**.
7. After the loop completes, the final sum is compared with the original input number using an *Equal?* function.
8. A **Select** function displays **"Perfect Number"** if both values are equal; otherwise, it displays **"Not a Perfect Number"**.

---

## Worked Examples

### Example 1: Input = 6 (Perfect)

- Loop count = `6 − 1 = 5`, so `i = 0, 1, 2, 3, 4` and divisors `d = 1, 2, 3, 4, 5`
- Sum initial value = 0

| Iteration (i) | Divisor d = i + 1 | R = 6 mod d | R = 0? | Sum |
|---|---|---|---|---|
| 0 | 1 | 0 | Yes | 0 + 1 = 1 |
| 1 | 2 | 0 | Yes | 1 + 2 = 3 |
| 2 | 3 | 0 | Yes | 3 + 3 = 6 |
| 3 | 4 | 2 | No | 6 |
| 4 | 5 | 1 | No | 6 |

Final comparison: `Sum = 6` and `Input = 6`, so `Equal?` is TRUE.

**Result: Sum = 6, "Perfect Number"**

### Example 2: Input = 9 (Not Perfect)

- Loop count = `9 − 1 = 8`, divisors `d = 1, 2, ..., 8`

| Iteration (i) | Divisor d | R = 9 mod d | R = 0? | Sum |
|---|---|---|---|---|
| 0 | 1 | 0 | Yes | 1 |
| 1 | 2 | 1 | No | 1 |
| 2 | 3 | 0 | Yes | 4 |
| 3 | 4 | 1 | No | 4 |
| 4 | 5 | 4 | No | 4 |
| 5 | 6 | 3 | No | 4 |
| 6 | 7 | 2 | No | 4 |
| 7 | 8 | 1 | No | 4 |

Final comparison: `Sum = 4` and `Input = 9`, so `Equal?` is FALSE.

**Result: Sum = 4, "Not a Perfect Number"**

### Example 3: Input = 28 (Perfect)

- Loop count = 27, divisors `d = 1` to `27`
- Divisors with remainder 0 are 1, 2, 4, 7 and 14, so the sum builds up as:

```text
1 → 3 → 7 → 14 → 28
```

Final comparison: `Sum = 28` and `Input = 28`, so `Equal?` is TRUE.

**Result: Sum = 28, "Perfect Number"**

### Example 4: Input = 12 (Not Perfect, Abundant)

- Loop count = 11, divisors `d = 1` to `11`
- Divisors with remainder 0 are 1, 2, 3, 4 and 6, so the sum builds up as:

```text
1 → 3 → 6 → 10 → 16
```

Final comparison: `Sum = 16` and `Input = 12`, so `Equal?` is FALSE.

**Result: Sum = 16, "Not a Perfect Number"**

### Example 5: Input = 1

- Loop count = `1 − 1 = 0`, so the For Loop does not execute.
- The shift register keeps its initial value, `Sum = 0`.
- `Sum = 0` and `Input = 1`, so `Equal?` is FALSE.

**Result: Sum = 0, "Not a Perfect Number"**

---

## Test Cases and Results

| Input Number | Sum of Proper Divisors | Expected Output |
|---|---|---|
| 6 | 6 | Perfect Number |
| 28 | 28 | Perfect Number |
| 12 | 16 | Not a Perfect Number |
| 9 | 4 | Not a Perfect Number |
| 1 | 0 | Not a Perfect Number |

---

## Learning Outcomes
- Understood proper divisors and the concept of perfect numbers.
- Used a For Loop with the iteration terminal to generate divisors from 1 to N − 1.
- Used the Quotient & Remainder and Equal? functions to test divisibility.
- Used a Case Structure to add a divisor to the sum only when it divides the number.
- Used a Shift Register to accumulate a running sum across iterations.
- Used the Equal? and Select functions to display a string result.

---

## Tools Used
- LabVIEW
- For Loop
- Shift Register
- Case Structure
- Quotient & Remainder function
- Equal? function
- Select function
- Increment / Add function
- Numeric Control, Numeric Indicator and String Indicator

---

## Conclusion
Successfully developed a LabVIEW VI that checks whether a number is a Perfect Number by testing every divisor from 1 to N − 1, accumulating the proper divisors in a shift register, and comparing their sum with the original number.
