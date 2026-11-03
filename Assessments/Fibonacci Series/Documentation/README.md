# Experiment 01: Fibonacci Series Generation using LabVIEW

## 1. Aim

To develop a LabVIEW program that generates the Fibonacci series for a user-specified number of terms using a For Loop and Shift Registers.

## 2. Objective

- Understand the mathematical concept of the Fibonacci sequence.
- Implement iterative calculations using LabVIEW.
- Use a For Loop to execute a fixed number of iterations.
- Understand the working of Shift Registers for storing and updating values between loop iterations.
- Use an Array Indicator to display the generated Fibonacci sequence.

## 3. Theory

The Fibonacci series is a sequence of numbers in which each number is obtained by adding the two preceding numbers. The sequence starts with 0 and 1.

**Example:**

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...
```

The mathematical representation of the Fibonacci sequence is:

$$F(n) = F(n-1) + F(n-2)$$

With the initial conditions:

$$F(0) = 0$$

$$F(1) = 1$$

Each subsequent term is calculated by adding the previous two terms.

## 4. Algorithm

1. Start the program.
2. Accept the number of terms from the user through a Numeric Control.
3. Initialize two variables:
   - `A = 0`
   - `B = 1`
4. Execute a For Loop for the specified number of terms.
5. During each iteration:
   - Output the current value of `A`.
   - Calculate the sum of `A` and `B`.
   - Update `A` with the previous value of `B`.
   - Update `B` with the calculated sum.
6. Use the For Loop output tunnel with auto-indexing to collect the generated values into an array.
7. Display the Fibonacci sequence through the Array Indicator.
8. Stop the program.

## 5. Mathematical Logic

The program uses two values to generate the sequence. Initially:

$$A = 0, \qquad B = 1$$

The next value is calculated as:

$$C = A + B$$

The values are updated after every iteration:

$$A_{\text{new}} = B_{\text{old}}$$

$$B_{\text{new}} = A_{\text{old}} + B_{\text{old}}$$

**Example for 7 terms:**

| Iteration | A | B | A + B | Output |
|:---------:|--:|--:|------:|-------:|
| 0 | 0 | 1 | 1 | 0 |
| 1 | 1 | 1 | 2 | 1 |
| 2 | 1 | 2 | 3 | 1 |
| 3 | 2 | 3 | 5 | 2 |
| 4 | 3 | 5 | 8 | 3 |
| 5 | 5 | 8 | 13 | 5 |
| 6 | 8 | 13 | 21 | 8 |

Therefore, the output is:

**0, 1, 1, 2, 3, 5, 8**

## 6. LabVIEW Implementation

The program was developed using the following LabVIEW components.

### Front Panel

**1. Number of Terms – Numeric Control**

- Accepts the number of Fibonacci terms to be generated.
- Configured with I64 numeric representation.

**2. Fibonacci Series – Array Indicator**

- Displays the generated Fibonacci sequence.
- Contains a numeric indicator configured with I64 representation.
- Displays all generated terms as an array.

### Block Diagram

**1. For Loop**

- Executes the Fibonacci calculation for the specified number of terms.
- The `N` terminal receives the value from the Number of Terms control.

**2. Shift Registers**

Two Shift Registers are used to store the previous two Fibonacci numbers.

- First Shift Register: initialized with `0`.
- Second Shift Register: initialized with `1`.

The Shift Registers retain the values between successive loop iterations.

**3. Add Function**

- Adds the values stored in the two Shift Registers.
- Calculates the next Fibonacci number.

**4. Auto-Indexing Output Tunnel**

- Collects one Fibonacci number during every loop iteration.
- Automatically builds an output array.

**5. Array Indicator**

- Receives the generated array from the For Loop.
- Displays the complete Fibonacci sequence on the Front Panel.

## 7. Test Case

| | Value |
|---|---|
| **Input** | Number of Terms = 7 |
| **Expected Output** | 0, 1, 1, 2, 3, 5, 8 |
| **Actual Output** | 0, 1, 1, 2, 3, 5, 8 |

**Result:** The actual output matches the expected output.

## 8. Screenshots

### Front Panel

The Front Panel contains the Number of Terms control and Fibonacci Series array indicator.

![Front Panel](Screenshots/Front_Panel.png)

### Block Diagram

The Block Diagram shows the For Loop, two Shift Registers, Add Function and auto-indexing output tunnel.

![Block Diagram](Screenshots/Block_Diagram.png)

### Output

The output demonstrates the generated Fibonacci sequence for seven terms.

![Output](Screenshots/Output.png)

## 9. Learning Outcomes

Through this experiment, the following LabVIEW concepts were understood:

- Implementation of mathematical algorithms using LabVIEW.
- Working with For Loops for repeated execution.
- Understanding Shift Registers and how they retain data between loop iterations.
- Updating and transferring values between Shift Registers.
- Using the Add Function for arithmetic operations.
- Understanding auto-indexing in For Loops.
- Creating and displaying numeric arrays using Array Indicators.
- Selecting appropriate numeric representations for integer calculations.

## 10. Conclusion

The Fibonacci Series Generation program was successfully implemented in LabVIEW using a For Loop, two Shift Registers and an Add Function.

The program accepts the required number of terms from the user, generates the Fibonacci sequence through iterative calculations and displays the results in an Array Indicator.

This experiment provided practical understanding of loops, Shift Registers, arithmetic operations and array generation in LabVIEW.

## 11. Limitations

- The program is intended for non-negative numbers of terms.
- With the I64 representation, the largest value that fits is $2^{63}-1 = 9{,}223{,}372{,}036{,}854{,}775{,}807$. Since $F(92) = 7{,}540{,}113{,}804{,}746{,}346{,}429$ fits but $F(93)$ does not, and the loop computes one term ahead on every iteration, the number of terms should be limited to **91** to avoid integer overflow.
- Input validation for the allowed range can be added as an improvement.
