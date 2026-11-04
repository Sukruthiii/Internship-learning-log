# Experiment 04 – Factorial

## Aim
To create a VI that calculates the factorial of an entered non-negative integer and displays the result using a Numeric Indicator.

---

## Theory

### What is a factorial?
The **factorial** of a non-negative integer `n`, written `n!`, is the product of all positive integers from 1 up to `n`.

```text
n! = n × (n − 1) × (n − 2) × ... × 2 × 1
```

### Important facts
- **0! = 1** by definition (this is the "empty product" and keeps formulas such as permutations and combinations consistent).
- **1! = 1.**
- Factorial is defined only for **non-negative integers**. Negative numbers have no factorial, so they are treated as invalid input.
- Factorials grow extremely fast, as shown below.
- **Recursive definition:** `n! = n × (n − 1)!`

| n | n! |
|---|---|
| 0 | 1 |
| 1 | 1 |
| 2 | 2 |
| 3 | 6 |
| 4 | 24 |
| 5 | 120 |
| 6 | 720 |
| 7 | 5040 |

### Applications
- **Permutations:** number of ways to arrange `n` objects = `n!`
- **Combinations:** `C(n, r) = n! / (r! × (n − r)!)`
- Probability, series expansions (e.g. Taylor series), and algorithm analysis.

### Iterative method used in the VI
Instead of recursion, the VI uses a loop with a running product:

```text
Result = 1
For i = 0 to N − 1:
    multiplier = N − i
    Result = Result × multiplier
```

The multipliers produced are `N, N−1, N−2, ..., 1`, so the final Result equals `N!`.

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Validity check | `Input < 0` → Not Valid |
| Loop count | `N = Input Number` |
| Iteration index | `i = 0, 1, 2, ..., N − 1` |
| Multiplier | `Multiplier = N − i` (ranges from N down to 1) |
| Shift register initial value | `1` |
| Update | `Result(new) = Result(previous) × Multiplier` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number | Numeric Control | Enter the non-negative integer |
| Factorial Result | Numeric Indicator | Displays the factorial of the input |
| Not Valid? | Boolean LED | Turns ON when the input is negative |

![Front Panel](images/factorial_front_panel.png)

---

## Block Diagram Implementation

1. **Input Validation:** A *Less Than?* function checks whether the input number is less than 0. If TRUE, the input is classified as invalid.
2. **Case Structure:** The Boolean comparison output is connected to the Case Structure selector.
   - **TRUE:** The input is invalid. The factorial calculation is skipped and the result is set to **0**.
   - **FALSE:** The input is valid and the factorial calculation is performed.
3. **For Loop:** A For Loop performs the repeated multiplication required to calculate the factorial.
4. **Loop Count (N):** The input number is connected to the N terminal of the For Loop, determining the number of iterations.
5. **Shift Register:** A shift register stores the intermediate factorial result between iterations. It is initialized to **1**.
6. **Multiplier Generation:** The iteration terminal (`i`) is subtracted from the input number (`N − i`) to generate the multiplier for each iteration.
7. **Multiplication:** A *Multiply* function multiplies the previous shift register value by the current multiplier.
8. **Result Storage:** The multiplication output is connected to the right shift register terminal, which stores the updated product for the next iteration.
9. **Final Result:** After the loop completes, the final shift register value is passed through the Case Structure output tunnel to the Factorial Result Numeric Indicator.
10. **Boolean LED:** The *Not Valid?* LED turns ON when the input is negative and remains OFF for valid non-negative inputs.

### Block Diagram – Input less than 0 (TRUE case)
![Block Diagram TRUE](images/factorial_block_true.png)

### Block Diagram – Input greater than or equal to 0 (FALSE case)
![Block Diagram FALSE](images/factorial_block_false.png)

---

## Worked Examples

### Example 1: Input = 5

- Input ≥ 0, so the FALSE case runs.
- Loop count: `N = 5`, so `i = 0, 1, 2, 3, 4`
- Shift register initial value = 1

| Iteration (i) | Multiplier = N − i | Previous Value | New Value = Previous × Multiplier |
|---|---|---|---|
| 0 | 5 − 0 = 5 | 1 | 1 × 5 = 5 |
| 1 | 5 − 1 = 4 | 5 | 5 × 4 = 20 |
| 2 | 5 − 2 = 3 | 20 | 20 × 3 = 60 |
| 3 | 5 − 3 = 2 | 60 | 60 × 2 = 120 |
| 4 | 5 − 4 = 1 | 120 | 120 × 1 = 120 |

**Result: 5! = 120**, Not Valid? LED = OFF

### Example 2: Input = 4

- Loop count: `N = 4`, so `i = 0, 1, 2, 3`

| Iteration (i) | Multiplier = N − i | Previous Value | New Value |
|---|---|---|---|
| 0 | 4 | 1 | 4 |
| 1 | 3 | 4 | 12 |
| 2 | 2 | 12 | 24 |
| 3 | 1 | 24 | 24 |

**Result: 4! = 24**, Not Valid? LED = OFF

### Example 3: Input = 0

- Loop count: `N = 0`, so the For Loop does **not execute at all**.
- The shift register keeps its initial value of 1.

**Result: 0! = 1**, Not Valid? LED = OFF

### Example 4: Input = −1

- `Input < 0` is TRUE, so the Case Structure runs the TRUE case.
- The factorial calculation is skipped.

**Result: 0**, Not Valid? LED = **ON**

---

## Test Cases and Results

Enter the number, run the VI, and verify the output.

| Input No. | Expected Output | Not Valid? LED | LabVIEW Output |
|---|---|---|---|
| 0 | 1 | OFF | ![0](images/factorial_test_0.png) |
| 5 | 120 | OFF | ![5](images/factorial_test_5.png) |
| −1 | Not Valid (Result = 0) | ON | ![-1](images/factorial_test_neg1.png) |

---

## Note on Data Type
Factorial values grow very quickly. If the indicator uses the **I32** data type, the largest factorial that fits is `12! = 479,001,600`, since `13!` exceeds the I32 limit. Using a **double-precision (DBL)** or **U64** representation allows larger inputs.

---

## Learning Outcomes
- Understood the concept of factorial and its special case 0! = 1.
- Learned to use a For Loop for repeated multiplication.
- Used a Shift Register to carry values between loop iterations.
- Used a Case Structure for input validation.
- Handled invalid (negative) input using a Boolean LED indicator.

---

## Tools Used
- LabVIEW
- For Loop
- Shift Register
- Case Structure
- Less Than? function
- Subtract function
- Multiply function
- Boolean LED Indicator
- Numeric Control and Indicator

---

## Conclusion
Successfully developed a LabVIEW VI to calculate the factorial of a non-negative integer using a For Loop with a shift register, with input validation that flags negative numbers through a Boolean LED.
