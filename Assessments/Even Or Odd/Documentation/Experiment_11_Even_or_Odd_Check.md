# Experiment 11 – Even or Odd Check

## Aim
To create a VI that checks whether the entered integer is even or odd and displays the result using a String Indicator.

---

## Theory

### Even and odd numbers
- An **even number** is an integer that is exactly divisible by 2, so dividing it by 2 leaves **remainder 0**. Examples: −4, 0, 2, 8, 100.
- An **odd number** is an integer that is **not** exactly divisible by 2, so dividing it by 2 leaves a **non-zero remainder** (the remainder is 1 in magnitude). Examples: −3, 1, 7, 99.

Every integer can be written in one of two forms:

```text
Even : n = 2k        (remainder 0)
Odd  : n = 2k + 1    (remainder 1)
```

where `k` is an integer.

### Division formula
When an integer is divided by another, the result follows the **division algorithm**:

```text
Dividend = (Divisor × Quotient) + Remainder
```

For this VI the divisor is always 2:

```text
Input Number = (2 × Quotient) + Remainder
```

### Decision rule
| Remainder (Input mod 2) | Result |
|---|---|
| 0 | Even |
| Not 0 | Odd |

### Select function
The **Select** function takes a Boolean input and two data inputs. It outputs one value if the Boolean is TRUE and the other if it is FALSE:

```text
Output = (Condition is TRUE) ? "t" input : "f" input
```

Here the TRUE input is wired to the string `"Even"` and the FALSE input to the string `"Odd"`.

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Remainder | `Remainder = Input Number mod 2` |
| Comparison | `Is Even? = (Remainder = 0)` |
| Output string | `Result = "Even"` if `Is Even?` is TRUE, else `"Odd"` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Number | Numeric Control | Enter the integer to be checked |
| Result | String Indicator | Displays whether the number is Even or Odd |

---

## Block Diagram Implementation

1. The *Quotient & Remainder* function divides the input number by 2 and calculates the remainder.
2. The *Equal?* function compares the remainder with 0.
3. The *Select* function uses the Boolean comparison result to select the appropriate string output.
4. If the remainder is 0, the output is **"Even"**; otherwise, the output is **"Odd"**.
5. The selected result is displayed through the String Indicator.

---

## Worked Examples

### Example 1: Input = 8

```text
8 = (2 × 4) + 0
```

| Step | Value |
|---|---|
| Quotient | 4 |
| Remainder | 0 |
| Remainder = 0? | TRUE |

**Result: "Even"**

### Example 2: Input = 7

```text
7 = (2 × 3) + 1
```

| Step | Value |
|---|---|
| Quotient | 3 |
| Remainder | 1 |
| Remainder = 0? | FALSE |

**Result: "Odd"**

### Example 3: Input = 0

```text
0 = (2 × 0) + 0
```

| Step | Value |
|---|---|
| Quotient | 0 |
| Remainder | 0 |
| Remainder = 0? | TRUE |

**Result: "Even"** (zero is an even number)

### Example 4: Negative numbers

| Input | Remainder after dividing by 2 | Remainder = 0? | Result |
|---|---|---|---|
| −4 | 0 | TRUE | Even |
| −3 | Non-zero | FALSE | Odd |

Only whether the remainder is zero matters, so the sign of the input does not affect the result and negative integers are classified correctly.

---

## Test Cases and Results

| Input Number | Remainder | Expected Output |
|---|---|---|
| 8 | 0 | Even |
| 7 | 1 | Odd |
| 0 | 0 | Even |
| −4 | 0 | Even |
| −3 | Non-zero | Odd |

---

## Learning Outcomes
- Understood the concept of even and odd numbers using divisibility by 2.
- Used the Quotient & Remainder function to find a remainder.
- Used the Equal? function to compare the remainder with zero.
- Used the Select function to choose between two string outputs based on a Boolean condition.
- Displayed the result using a String Indicator.

---

## Tools Used
- LabVIEW
- Quotient & Remainder function
- Equal? function
- Select function
- String Constants ("Even", "Odd")
- Numeric Control and String Indicator

---

## Conclusion
Successfully developed a LabVIEW VI that determines whether an entered integer is even or odd by checking the remainder of division by 2, and displays the result as a string.
