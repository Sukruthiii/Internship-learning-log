# Experiment – Prime Factors of a Number

## Aim
To develop a LabVIEW VI that finds and displays all the prime factors of a given positive integer using a While Loop and Shift Registers.

---

## Theory

### Prime factors
A **prime number** is a positive integer greater than 1 whose only factors are 1 and itself (2, 3, 5, 7, 11, ...).

A **prime factor** of a number is a prime number that divides it exactly. **Prime factorization** writes a number as a product of primes.

```text
60 = 2 × 2 × 3 × 5 = 2² × 3 × 5
```

### Fundamental Theorem of Arithmetic
Every integer greater than 1 is either prime or can be written as a **product of primes in exactly one way** (ignoring the order of the factors). So the list of prime factors of a number is unique.

### Repeated division method
The prime factors are found by dividing the number by the smallest possible divisor again and again:

1. Start with divisor `d = 2`.
2. If `d` divides the remaining number exactly (remainder 0), then `d` is a prime factor. Store it and replace the remaining number with the quotient. **Keep the same divisor**, because the same prime may divide the number again.
3. If `d` does not divide the remaining number, increase `d` by 1.
4. Stop when the remaining number becomes 1.

Using the division formula `Dividend = (Divisor × Quotient) + Remainder`, each successful step looks like:

```text
Remaining = (d × Quotient) + 0
```

### Why every divisor found is prime
The divisor starts at 2 and only moves up when it no longer divides the remaining number. By the time the divisor reaches a composite number such as 4 or 6, all of its smaller prime factors (2, 3) have already been divided out completely, so the composite number can never divide the remaining value. Therefore only primes are ever stored in the array.

### Shift registers
Three shift registers carry values from one iteration to the next:

| Shift Register | Stores | Initial value |
|---|---|---|
| Remaining number | The number still to be factorized | Input Number |
| Divisor | The current divisor being tested | 2 |
| Prime factors array | The prime factors found so far | Empty array |

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Remainder | `R = Remaining mod Divisor` |
| Divisibility test | `R = 0` |
| If `R = 0` (TRUE case) | `Factors (new) = Build Array(Factors, Divisor)`, `Remaining (new) = Remaining ÷ Divisor`, `Divisor` unchanged |
| If `R ≠ 0` (FALSE case) | `Divisor (new) = Divisor + 1`, `Remaining` and `Factors` unchanged |
| Stop condition | `Remaining = 1` |
| Check | Product of all factors in the array = Input Number |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number | Numeric Control | Enter the positive integer |
| Prime Factors | Numeric Array Indicator | Displays all the prime factors of the entered number |

---

## Block Diagram Implementation

1. A **While Loop** is used to repeatedly divide the input number until all its prime factors are extracted.
2. **Three Shift Registers** are used to store the remaining number, current divisor, and array of prime factors.
3. The divisor is initialized to **2**, and the prime factors array is initialized as an **empty array**.
4. The *Quotient & Remainder* function checks whether the remaining number is exactly divisible by the current divisor.
5. A **Case Structure** is used to implement the division logic:
   - **TRUE Case:** If the remainder is zero, the current divisor is added to the prime factors array using the *Build Array* function. The remaining number is updated with the quotient, while the divisor remains unchanged.
   - **FALSE Case:** If the remainder is non-zero, the divisor is incremented by 1. The remaining number and prime factors array retain their previous values.
6. The While Loop terminates when the remaining number becomes **1**.
7. The final array containing all the prime factors is displayed using the Prime Factors Array Indicator.

---

## Worked Examples

### Example 1: Input = 60

- Initial values: `Remaining = 60`, `Divisor = 2`, `Factors = [ ]`

| Iteration | Remaining | Divisor | R = Remaining mod Divisor | Case | Remaining (new) | Divisor (new) | Factors (new) |
|---|---|---|---|---|---|---|---|
| 1 | 60 | 2 | 0 | TRUE | 30 | 2 | [2] |
| 2 | 30 | 2 | 0 | TRUE | 15 | 2 | [2, 2] |
| 3 | 15 | 2 | 1 | FALSE | 15 | 3 | [2, 2] |
| 4 | 15 | 3 | 0 | TRUE | 5 | 3 | [2, 2, 3] |
| 5 | 5 | 3 | 2 | FALSE | 5 | 4 | [2, 2, 3] |
| 6 | 5 | 4 | 1 | FALSE | 5 | 5 | [2, 2, 3] |
| 7 | 5 | 5 | 0 | TRUE | **1** | 5 | [2, 2, 3, 5] |

The remaining number is now 1, so the loop stops.

**Result: [2, 2, 3, 5]**, since `2 × 2 × 3 × 5 = 60`

### Example 2: Input = 36

- Initial values: `Remaining = 36`, `Divisor = 2`, `Factors = [ ]`

| Iteration | Remaining | Divisor | R | Case | Remaining (new) | Divisor (new) | Factors (new) |
|---|---|---|---|---|---|---|---|
| 1 | 36 | 2 | 0 | TRUE | 18 | 2 | [2] |
| 2 | 18 | 2 | 0 | TRUE | 9 | 2 | [2, 2] |
| 3 | 9 | 2 | 1 | FALSE | 9 | 3 | [2, 2] |
| 4 | 9 | 3 | 0 | TRUE | 3 | 3 | [2, 2, 3] |
| 5 | 3 | 3 | 0 | TRUE | **1** | 3 | [2, 2, 3, 3] |

**Result: [2, 2, 3, 3]**, since `2 × 2 × 3 × 3 = 36`

### Example 3: Input = 13 (Prime number)

- For divisors 2 to 12, the remainder is never 0, so the FALSE case runs 11 times and the divisor increases from 2 to 13.
- At `Divisor = 13`: `R = 13 mod 13 = 0`, so the TRUE case runs, 13 is added to the array, and `Remaining = 13 ÷ 13 = 1`.

**Result: [13]**, because a prime number has only itself as its prime factor.

### Example 4: Input = 1

- `Remaining = 1` from the start, so the loop stops after its first execution (a While Loop always runs at least once) without storing any factor.

**Result: [ ]** (empty array), because 1 has no prime factors.

---

## Test Cases and Results

| Input Number | Expected Prime Factors |
|---|---|
| 60 | 2, 2, 3, 5 |
| 36 | 2, 2, 3, 3 |
| 13 | 13 |
| 1 | (none) |

---

## Note
The input must be a **positive integer**. For 0, the remaining number would never reach 1 (since `0 ÷ 2 = 0` repeatedly), so the loop would never stop. Negative numbers are also not valid inputs for this VI.

---

## Learning Outcomes
- Understood prime factors and prime factorization.
- Used a While Loop with a conditional stop based on the remaining number reaching 1.
- Used three Shift Registers to carry the remaining number, divisor and factor array between iterations.
- Used the Quotient & Remainder function to test divisibility.
- Used a Case Structure to decide between recording a factor and moving to the next divisor.
- Used the Build Array function to grow an array inside a loop.

---

## Tools Used
- LabVIEW
- While Loop
- Shift Registers
- Case Structure
- Quotient & Remainder function
- Build Array function
- Increment function
- Numeric Control and Numeric Array Indicator

---

## Conclusion
Successfully developed a LabVIEW VI that finds all the prime factors of a positive integer by repeatedly dividing by the smallest possible divisor inside a While Loop, using shift registers to carry the remaining number, the divisor and the array of factors.
