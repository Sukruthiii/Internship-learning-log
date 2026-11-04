# Experiment 03 – Prime Number Check

## Aim
To develop a LabVIEW VI that checks whether an entered integer is a prime number and displays the result using a String Indicator.

---

## Theory

### What is a prime number?
A **prime number** is a natural number greater than 1 that has exactly two positive factors: **1 and itself**.

A natural number greater than 1 that has more than two factors is called a **composite number**.

| Number | Factors | Type |
|---|---|---|
| 2 | 1, 2 | Prime |
| 7 | 1, 7 | Prime |
| 9 | 1, 3, 9 | Composite |
| 12 | 1, 2, 3, 4, 6, 12 | Composite |

### Important facts
- **0 and 1 are neither prime nor composite.** 1 has only one factor, so it fails the "exactly two factors" rule.
- **2 is the smallest prime and the only even prime.** Every other even number is divisible by 2.
- Negative numbers are not considered prime.
- The first few primes are: 2, 3, 5, 7, 11, 13, 17, 19, 23, 29, ...
- **Fundamental Theorem of Arithmetic:** every integer greater than 1 is either prime or can be written as a unique product of primes (e.g. 12 = 2 × 2 × 3).
- Primes are widely used in **cryptography** (such as RSA), hashing, and error-correcting codes.

### Divisibility and the division formula
Whenever an integer is divided by another, the result follows the **division algorithm**:

```text
Dividend = (Divisor × Quotient) + Remainder
```

For the VI, this is written as:

```text
Input Number = (Divisor × Quotient) + Remainder
```

The **Quotient & Remainder** function in LabVIEW computes both Quotient and Remainder from the Input Number and the Divisor.

- If **Remainder = 0**, the divisor divides the number exactly, so it is a factor.
- If **Remainder ≠ 0**, the divisor is not a factor.

### Prime test by trial division
A number is prime if **no number between 2 and (Input − 1)** divides it exactly. In other words:

```text
Input is Prime    if   (Input mod d) ≠ 0   for every d = 2, 3, ..., (Input − 1)
Input is Not Prime if  (Input mod d) = 0   for at least one such d
```

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Loop count | `N = Input Number − 2` |
| Iteration index | `i = 0, 1, 2, ..., N − 1` |
| Divisor | `Divisor = i + 2` (ranges from 2 to Input − 1) |
| Remainder | `Remainder = Input Number mod Divisor` |
| Stop condition | `Remainder = 0` |

---

## Description
This VI checks whether the entered number is prime by testing its divisibility using a **For Loop** and the **Quotient & Remainder** function.

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number | Numeric Control (I32) | Enter the integer to be tested |
| Result | String Indicator | Displays whether the number is prime or not |

---

## Block Diagram Implementation

1. **Input Validation:** A *Greater Or Equal?* function checks whether the input number is greater than or equal to 2. Numbers less than 2 are directly classified as **not prime**.
2. **Special Case – Number 2:** An *Equal?* function checks whether the input is exactly 2. If TRUE, the result is **"It is a Prime Number"**.
3. **For Loop:** For numbers greater than 2, a For Loop checks possible divisors starting from 2.
4. **Loop Count (N):** The count terminal is connected to `Input Number − 2`, because the divisors range from 2 to `Input Number − 1`.
5. **Divisor Generation:** The iteration terminal (`i`) is added to 2 to generate the divisor for each iteration.
6. **Divisibility Check:** The *Quotient & Remainder* function calculates the remainder when the input number is divided by the current divisor.
7. **Case Structure:** An *Equal?* function compares the remainder with zero.
   - **TRUE:** A divisor is found, so the number is **not prime**.
   - **FALSE:** The current divisor does not divide the number evenly, so the number **may be prime**.
8. **Conditional Loop Stop:** The For Loop uses a conditional terminal to stop execution when a divisor is found.
9. **String Output:** The result is passed through the Case Structure output tunnel to the String Indicator.

---

## Worked Examples

### Example 1: Input = 7 (Prime)

- Input ≥ 2 and Input ≠ 2, so the For Loop is used.
- Loop count: `N = 7 − 2 = 5`, so `i = 0, 1, 2, 3, 4`

| Iteration (i) | Divisor = i + 2 | Quotient | Remainder = 7 mod Divisor | Remainder = 0? |
|---|---|---|---|---|
| 0 | 2 | 3 | 1 | No |
| 1 | 3 | 2 | 1 | No |
| 2 | 4 | 1 | 3 | No |
| 3 | 5 | 1 | 2 | No |
| 4 | 6 | 1 | 1 | No |

Verification using the division formula for `i = 1`:
`7 = (3 × 2) + 1`

The remainder is never zero, so the loop completes all 5 iterations.
**Result: "It is a Prime Number"**

### Example 2: Input = 9 (Not Prime)

- Loop count: `N = 9 − 2 = 7`, so `i = 0, 1, ..., 6`

| Iteration (i) | Divisor = i + 2 | Quotient | Remainder = 9 mod Divisor | Remainder = 0? |
|---|---|---|---|---|
| 0 | 2 | 4 | 1 | No |
| 1 | 3 | 3 | 0 | **Yes → Stop** |

Verification using the division formula for `i = 1`:
`9 = (3 × 3) + 0`

The remainder is zero at `i = 1`, so the loop stops after only 2 iterations (the remaining 5 are skipped).
**Result: "Not a Prime Number"**

### Example 3: Input = 12 (Not Prime, stops immediately)

- Loop count: `N = 12 − 2 = 10`

| Iteration (i) | Divisor = i + 2 | Quotient | Remainder = 12 mod Divisor | Remainder = 0? |
|---|---|---|---|---|
| 0 | 2 | 6 | 0 | **Yes → Stop** |

`12 = (2 × 6) + 0`

The loop stops in the very first iteration.
**Result: "Not a Prime Number"**

### Example 4: Special cases (no loop needed)

| Input | Check | Result |
|---|---|---|
| 1 | Input ≥ 2 is FALSE | Not a Prime Number |
| 0 | Input ≥ 2 is FALSE | Not a Prime Number |
| −5 | Input ≥ 2 is FALSE | Not a Prime Number |
| 2 | Input = 2 is TRUE | It is a Prime Number |

---

## Test Cases

| Input Number | Expected Result |
|---|---|
| 7 | Prime Number |
| 9 | Not a Prime Number |

---

## Learning Outcomes
- Understood the concept of prime numbers and divisibility.
- Learned to use For Loops for repeated divisor checking.
- Practiced Quotient & Remainder operations.
- Used Case Structures for decision-making.
- Learned to use conditional loop termination.
- Understood how to handle special cases and invalid prime-number inputs.

---

## Tools Used
- LabVIEW
- For Loop
- Case Structure
- Greater Or Equal? function
- Equal? function
- Quotient & Remainder function
- Conditional Terminal

---

## Conclusion
Successfully developed a LabVIEW VI to identify prime and non-prime numbers using loop-based divisor checking and conditional logic.
