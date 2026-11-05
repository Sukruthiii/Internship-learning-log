# Experiment 07 – Sum of Digits

## Aim
To create a VI that calculates the sum of the digits of a given integer and displays the result using a Numeric Indicator.

---

## Theory

### What is the sum of digits?
The **sum of digits** of an integer is the total obtained by adding all of its individual digits.

```text
Sum of digits of 1234 = 1 + 2 + 3 + 4 = 10
```

### Extracting digits using division by 10
In the decimal system, dividing a number by 10 separates its last digit from the rest of the number:

- The **remainder** (`number mod 10`) is the **last digit**.
- The **integer quotient** (`number ÷ 10`, ignoring the fractional part) is the number with the **last digit removed**.

Using the division formula `Dividend = (Divisor × Quotient) + Remainder`:

```text
1234 = (10 × 123) + 4
        Quotient = 123  (remaining number)
        Remainder = 4   (last digit)
```

Repeating this on the quotient extracts every digit, from the last to the first, until the quotient becomes 0.

| Step | Number | Remainder (digit) | Quotient (remaining number) |
|---|---|---|---|
| 1 | 1234 | 4 | 123 |
| 2 | 123 | 3 | 12 |
| 3 | 12 | 2 | 1 |
| 4 | 1 | 1 | 0 |

### Handling negative numbers
The sum of digits depends only on the digits and not on the sign. The **Absolute Value** function converts a negative input to positive first, so −507 is processed as 507.

### While Loop and shift registers
- A **While Loop** repeats until a stop condition is met. It always runs **at least once**, which is useful here because even the input 0 must be processed.
- A **shift register** carries a value from one iteration to the next. Two are used:
  - **Sum:** accumulates the digits, initialized to 0.
  - **Temp:** holds the number still to be processed, initialized to the absolute value of the input.

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Working number | `Temp (initial) = \|Input Number\|` |
| Last digit | `R = Temp mod 10` |
| Remaining number | `IQ = Temp ÷ 10` (integer quotient) |
| Sum update | `Sum (new) = Sum (previous) + R` |
| Temp update | `Temp (new) = IQ` |
| Stop condition | `IQ = 0` |
| Sum shift register initial value | `0` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number | Numeric Control | Enter the integer |
| Sum of Digits | Numeric Indicator | Displays the sum of the digits |

---

## Block Diagram Implementation

1. **Absolute Value:** An *Absolute Value* function is used to convert negative input numbers into positive numbers so that negative integers can also be processed.
2. **While Loop:** A While Loop repeatedly extracts individual digits from the input number until the quotient becomes zero.
3. **Shift Registers:** Two shift registers are used:
   - **Sum:** Initialized to **0** to store the accumulated sum of the digits.
   - **Temp:** Stores the current number being processed.
4. **Digit Extraction:** The *Quotient & Remainder* function divides the current number by 10.
   - **Remainder (R)** extracts the last digit.
   - **Integer Quotient (IQ)** removes the last digit from the number.
5. **Sum Calculation:** An *Add* function adds the extracted digit to the previous sum stored in the Sum shift register.
6. **Number Update:** The quotient is passed back to the Temp shift register for the next iteration.
7. **Loop Termination:** An *Equal to Zero?* function checks whether the updated quotient is zero. When the quotient becomes zero, the While Loop stops executing.
8. **Output:** The final accumulated sum is displayed in the Sum of Digits Numeric Indicator.

---

## Worked Examples

### Example 1: Input = 1234

- `Temp = |1234| = 1234`, `Sum = 0`

| Iteration | Temp | R = Temp mod 10 | IQ = Temp ÷ 10 | Sum = Sum + R | IQ = 0? |
|---|---|---|---|---|---|
| 1 | 1234 | 4 | 123 | 0 + 4 = 4 | No |
| 2 | 123 | 3 | 12 | 4 + 3 = 7 | No |
| 3 | 12 | 2 | 1 | 7 + 2 = 9 | No |
| 4 | 1 | 1 | 0 | 9 + 1 = 10 | **Yes → Stop** |

**Result: 10**

### Example 2: Input = −507 (Negative number)

- `Temp = |−507| = 507`, `Sum = 0`

| Iteration | Temp | R | IQ | Sum | IQ = 0? |
|---|---|---|---|---|---|
| 1 | 507 | 7 | 50 | 0 + 7 = 7 | No |
| 2 | 50 | 0 | 5 | 7 + 0 = 7 | No |
| 3 | 5 | 5 | 0 | 7 + 5 = 12 | **Yes → Stop** |

**Result: 12**

### Example 3: Input = 0

- `Temp = 0`, `Sum = 0`
- The While Loop runs once: `R = 0 mod 10 = 0`, `IQ = 0`, `Sum = 0 + 0 = 0`, and `IQ = 0` is TRUE so the loop stops.

**Result: 0**

### Example 4: Input = 9 (Single digit)

- Iteration 1: `R = 9`, `IQ = 0`, `Sum = 0 + 9 = 9`, loop stops.

**Result: 9**

---

## Test Cases and Results

| Input Number | Expected Output |
|---|---|
| 1234 | 10 |
| −507 | 12 |
| 9 | 9 |
| 0 | 0 |

---

## Learning Outcomes
- Understood how digits are extracted from a number using division and remainder by 10.
- Used the Quotient & Remainder function to separate the last digit from the rest of the number.
- Used the Absolute Value function to handle negative inputs.
- Used a While Loop with a conditional stop based on the quotient becoming zero.
- Used two shift registers to carry the running sum and the remaining number between iterations.

---

## Tools Used
- LabVIEW
- While Loop
- Shift Registers
- Absolute Value function
- Quotient & Remainder function
- Add function
- Equal to Zero? function
- Numeric Control and Numeric Indicator

---

## Conclusion
Successfully developed a LabVIEW VI that calculates the sum of the digits of an integer by repeatedly extracting the last digit with Quotient & Remainder inside a While Loop, accumulating the digits in a shift register, and handling negative inputs with the Absolute Value function.
