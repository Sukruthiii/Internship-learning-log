# Experiment 08 – Armstrong Number Check

## Aim
To create a VI that checks whether the entered non-negative integer is an Armstrong number and displays the result as a string output.

---

## Theory

### What is an Armstrong number?
An **Armstrong number** (also called a narcissistic number) is a number that equals the **sum of its own digits, each raised to the power of the number of digits**.

For a number with `n` digits `d₁ d₂ ... dₙ`:

```text
Number = d₁ⁿ + d₂ⁿ + ... + dₙⁿ
```

Example: `153` has 3 digits, so

```text
1³ + 5³ + 3³ = 1 + 125 + 27 = 153
```

Since the sum equals the original number, 153 is an Armstrong number.

### Examples of Armstrong numbers
| Digits | Armstrong numbers |
|---|---|
| 1 digit | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 (every single digit) |
| 3 digits | 153, 370, 371, 407 |
| 4 digits | 1634, 8208, 9474 |

### Finding the number of digits
The input number is converted to a **decimal string** using *Number to Decimal String*. The **String Length** of that string gives the number of digits `n`. For example, `153` becomes the string `"153"`, whose length is 3.

This value is used for two purposes:
- the **exponent** applied to every digit, and
- the **loop count** of the For Loop (one iteration per digit).

### Extracting digits using division by 10
Dividing by 10 separates the last digit from the rest of the number:

- **Remainder** (`Temp mod 10`) is the last digit.
- **Integer quotient** (`Temp ÷ 10`) is the number with its last digit removed.

```text
153 = (10 × 15) + 3
       Quotient = 15 (remaining number)
       Remainder = 3 (last digit)
```

### Shift registers
Two shift registers carry values from one iteration to the next:
- **Temp:** the number still to be processed, initialized with the original input.
- **Sum:** the running total of the powered digits, initialized to 0.

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Number of digits | `n = String Length(Number to Decimal String(Input))` |
| Loop count | `N = n` |
| Last digit | `R = Temp mod 10` |
| Remaining number | `IQ = Temp ÷ 10` (integer quotient) |
| Powered digit | `P = Rⁿ` (Power of X) |
| Sum update | `Sum (new) = Sum (previous) + P` |
| Temp update | `Temp (new) = IQ` |
| Temp initial value | `Input Number` |
| Sum initial value | `0` |
| Decision | `Armstrong if Sum = Input Number` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number | Numeric Control | Enter the non-negative integer |
| Result | String Indicator | Displays whether the number is an Armstrong number |

---

## Block Diagram Implementation

1. **Number to Decimal String:** Converts the input number into a decimal string.
2. **String Length:** Calculates the total number of digits in the input number. This value determines the exponent and the number of iterations in the For Loop.
3. **For Loop:** Executes once for each digit of the input number.
4. **Shift Registers:** Two shift registers are used:
   - **Temp:** Stores the current number being processed and is initialized with the original input number.
   - **Sum:** Stores the accumulated sum of the powered digits and is initialized to **0**.
5. **Digit Extraction:** The *Quotient & Remainder* function divides the current number by 10 to extract the last digit.
   - **Remainder (R)** extracts the last digit.
   - **Integer Quotient (IQ)** removes the last digit from the number.
6. **Power Calculation:** The *Power of X* function raises each extracted digit to the power of the total number of digits obtained from String Length.
7. **Sum Calculation:** An *Add* function adds the powered digit to the previous sum stored in the Sum shift register.
8. **Number Update:** The quotient is passed back to the Temp shift register for the next iteration.
9. **Comparison:** An *Equal?* function compares the final calculated sum with the original input number.
10. **String Output:** The Boolean result from the Equal? function is connected to a *Select* function.
    - **TRUE:** Displays **"Armstrong Number"**.
    - **FALSE:** Displays **"Not an Armstrong Number"**.

---

## Worked Examples

### Example 1: Input = 153 (Armstrong)

- `"153"` has `String Length = 3`, so `n = 3` and the loop count `N = 3`
- `Temp = 153`, `Sum = 0`

| Iteration | Temp | R = Temp mod 10 | IQ = Temp ÷ 10 | P = R³ | Sum = Sum + P |
|---|---|---|---|---|---|
| 1 | 153 | 3 | 15 | 3³ = 27 | 0 + 27 = 27 |
| 2 | 15 | 5 | 1 | 5³ = 125 | 27 + 125 = 152 |
| 3 | 1 | 1 | 0 | 1³ = 1 | 152 + 1 = 153 |

Comparison: `Sum = 153` and `Input = 153`, so `Equal?` is TRUE.

**Result: "Armstrong Number"**

### Example 2: Input = 123 (Not Armstrong)

- `n = 3`, `Temp = 123`, `Sum = 0`

| Iteration | Temp | R | IQ | P = R³ | Sum |
|---|---|---|---|---|---|
| 1 | 123 | 3 | 12 | 27 | 27 |
| 2 | 12 | 2 | 1 | 8 | 35 |
| 3 | 1 | 1 | 0 | 1 | 36 |

Comparison: `Sum = 36` and `Input = 123`, so `Equal?` is FALSE.

**Result: "Not an Armstrong Number"**

### Example 3: Input = 9474 (4-digit Armstrong)

- `"9474"` has `String Length = 4`, so `n = 4` and the exponent is 4.

| Iteration | Temp | R | IQ | P = R⁴ | Sum |
|---|---|---|---|---|---|
| 1 | 9474 | 4 | 947 | 4⁴ = 256 | 256 |
| 2 | 947 | 7 | 94 | 7⁴ = 2401 | 2657 |
| 3 | 94 | 4 | 9 | 4⁴ = 256 | 2913 |
| 4 | 9 | 9 | 0 | 9⁴ = 6561 | 9474 |

Comparison: `Sum = 9474` and `Input = 9474`, so `Equal?` is TRUE.

**Result: "Armstrong Number"**

### Example 4: Single-digit numbers

For `Input = 5`, `n = 1`, so `P = 5¹ = 5` and `Sum = 5`, which equals the input.

**Result: "Armstrong Number"** (every single-digit number, including 0, is an Armstrong number)

---

## Test Cases and Results

| Input Number | Number of Digits | Sum of Powered Digits | Expected Output |
|---|---|---|---|
| 153 | 3 | 153 | Armstrong Number |
| 123 | 3 | 36 | Not an Armstrong Number |
| 9474 | 4 | 9474 | Armstrong Number |
| 5 | 1 | 5 | Armstrong Number |

---

## Learning Outcomes
- Understood the concept of Armstrong numbers.
- Used Number to Decimal String and String Length to count the digits of a number.
- Used the Quotient & Remainder function to extract digits one at a time.
- Used the Power of X function to raise each digit to the power of the digit count.
- Used two shift registers to carry the remaining number and the running sum between iterations.
- Used the Equal? and Select functions to produce a string result.

---

## Tools Used
- LabVIEW
- For Loop
- Shift Registers
- Number to Decimal String function
- String Length function
- Quotient & Remainder function
- Power of X function
- Add function
- Equal? function
- Select function
- Numeric Control and String Indicator

---

## Conclusion
Successfully developed a LabVIEW VI that checks whether a number is an Armstrong number by extracting each digit, raising it to the power of the digit count, summing the results in a shift register, and comparing the sum with the original input.
