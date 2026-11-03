# Experiment 02: Palindrome Number Check

## Aim

To develop a LabVIEW VI that checks whether a given integer is a palindrome number using a While Loop, Shift Registers, and Quotient & Remainder operations.

## Objective

- Understand the logic behind checking palindrome numbers.
- Implement digit extraction using the Quotient & Remainder function.
- Use Shift Registers to store and update values across loop iterations.
- Reverse a number mathematically using multiplication and addition.
- Compare the original number with its reversed number to determine whether it is a palindrome.

## Theory

A palindrome number is a number that remains the same when its digits are reversed.

**Examples:**

| Number | Reversed | Result |
|-------:|---------:|--------|
| 121 | 121 | Palindrome |
| 1331 | 1331 | Palindrome |
| 123 | 321 | Not a palindrome |
| 10 | 1 | Not a palindrome |

### Mathematical Logic

The last digit of a number is extracted using:

$$\text{Digit} = \text{Number} \bmod 10$$

The remaining number is obtained using integer division:

$$\text{Temporary Number} = \left\lfloor \frac{\text{Number}}{10} \right\rfloor$$

The reversed number is constructed using:

$$\text{Reverse} = (\text{Reverse} \times 10) + \text{Digit}$$

After all digits have been processed, the original number is compared with the reversed number. If both values are equal, the number is a palindrome.

## Algorithm

1. Read an integer from the user.
2. Store the original number for comparison.
3. Initialize a temporary number with the input number.
4. Initialize the reversed number to zero.
5. Enter a While Loop.
6. Extract the last digit using Quotient & Remainder with divisor 10.
7. Multiply the current reversed number by 10 and add the extracted digit.
8. Update the temporary number with the quotient.
9. Repeat until the quotient becomes zero.
10. Compare the reversed number with the original number.
11. Display the Boolean result.

## LabVIEW Implementation

The VI is implemented using the following LabVIEW components:

| Component | Purpose |
|---|---|
| Numeric Control | Accepts the input number |
| While Loop | Repeats digit extraction until all digits are processed |
| Shift Register 1 | Stores the temporary number |
| Shift Register 2 | Stores the reversed number |
| Quotient & Remainder | Extracts the last digit and removes it from the temporary number |
| Multiply | Multiplies the reversed number by 10 |
| Add | Adds the extracted digit to the reversed number |
| Equal? | Checks whether the quotient is zero and compares the final numbers |
| Boolean LED | Displays whether the input is a palindrome |

### Shift Register Operation

Two Shift Registers are used:

- **Temporary Number:** Initialized with the original input and updated with the quotient in each iteration.
- **Reversed Number:** Initialized with zero and updated using the extracted digit.

The final reversed number is obtained from the right-side output of the While Loop Shift Register.

## Test Cases

| Input | Reversed Number | Expected Result |
|------:|----------------:|:---------------:|
| 121 | 121 | TRUE |
| 123 | 321 | FALSE |
| 1331 | 1331 | TRUE |
| 10 | 1 | FALSE |
| 7 | 7 | TRUE |
| 0 | 0 | TRUE |

## Learning Outcomes

- Understood how to reverse a number using mathematical operations.
- Learned to extract digits using Quotient & Remainder.
- Practiced using multiple Shift Registers in a While Loop.
- Understood how Shift Registers retain values between loop iterations.
- Implemented loop termination using a comparison condition.
- Applied Boolean comparison to identify palindrome numbers.

## Conclusion

Successfully developed a LabVIEW VI to check whether a given non-negative integer is a palindrome. The experiment provided practical experience with While Loops, Shift Registers, arithmetic functions, and comparison operations.
