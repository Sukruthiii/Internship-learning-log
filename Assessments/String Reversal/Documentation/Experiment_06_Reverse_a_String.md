# Experiment 06 – Reverse a String

## Aim
To develop a VI that takes a string as input, reverses the order of its characters, and displays the reversed string as output.

---

## Theory

### What is a string?
A **string** is a sequence of characters such as letters, digits, spaces and symbols. Each character occupies a position in the string, identified by an **index**.

- **Indexing starts from 0** in LabVIEW, so the first character is at index 0 and the last character is at index `Length − 1`.
- Spaces and special characters are also characters, so they are counted in the string length and are reversed along with the rest.

Example for the string `HELLO`:

| Index | 0 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|---|
| Character | H | E | L | L | O |

### String functions used
| Function | Purpose |
|---|---|
| String Length | Returns the total number of characters in a string |
| String Subset | Extracts a part of a string, given a starting **offset** (index) and a **length** |
| Concatenate Strings | Joins two or more strings in the order they are wired |

### Reversing a string
To reverse a string, the characters are read **from the last position to the first** and appended one by one to a new string.

```text
Original : H  E  L  L  O
Reversed : O  L  L  E  H
```

Since the loop counter `i` runs from 0 upwards, the index of the character to read is calculated so that it runs from the last position downwards:

```text
Index = String Length − 1 − i
```

### Shift Register as an accumulator
A **shift register** passes a value from one loop iteration to the next. Here it holds the reversed string built so far. It starts as an **empty string**, and each iteration appends one more character to it.

```text
New Reversed String = Previous Reversed String + Extracted Character
```

### Applications
- Palindrome checking (a word is a palindrome if it equals its reverse, e.g. MADAM)
- Text processing and data manipulation
- Basic encoding and puzzle-type algorithms

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Loop count | `N = String Length` |
| Iteration index | `i = 0, 1, 2, ..., N − 1` |
| Character index | `Index = String Length − 1 − i` (ranges from N − 1 down to 0) |
| Extracted character | `String Subset(Input, offset = Index, length = 1)` |
| Shift register initial value | Empty string |
| Update | `Reversed(new) = Reversed(previous) + Character` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input String | String Control | Enter the string to be reversed |
| Reversed String | String Indicator | Displays the reversed string |

---

## Block Diagram Implementation

1. **String Length:** The *String Length* function calculates the total number of characters in the input string.
2. **For Loop:** A For Loop iterates through the input string based on its length, executing once for each character.
3. **Index Calculation:** *Subtract* functions calculate the character index using the formula `Index = String Length − 1 − i`, allowing characters to be accessed from the last position to the first.
4. **String Subset:** The *String Subset* function extracts one character at a time from the input string using the calculated index. The length is set to **1**.
5. **Shift Register:** A Shift Register stores the previously accumulated reversed string and passes it to the next iteration. It is initialized with an **empty string**.
6. **Concatenate Strings:** The *Concatenate Strings* function combines the previously stored string with the newly extracted character during each iteration.
7. **Output:** After the For Loop completes, the final reversed string is connected to the String Indicator.

---

## Worked Examples

### Example 1: Input = `HELLO`

- `String Length = 5`, so the loop count `N = 5` and `i = 0, 1, 2, 3, 4`
- Shift register initial value = `""` (empty)

| Iteration (i) | Index = 5 − 1 − i | Extracted Character | Previous String | New String (Previous + Character) |
|---|---|---|---|---|
| 0 | 5 − 1 − 0 = 4 | O | "" | "O" |
| 1 | 5 − 1 − 1 = 3 | L | "O" | "OL" |
| 2 | 5 − 1 − 2 = 2 | L | "OL" | "OLL" |
| 3 | 5 − 1 − 3 = 1 | E | "OLL" | "OLLE" |
| 4 | 5 − 1 − 4 = 0 | H | "OLLE" | "OLLEH" |

**Result: `OLLEH`**

### Example 2: Input = `LabVIEW`

- `String Length = 7`, so `N = 7` and `i = 0, 1, ..., 6`

| Iteration (i) | Index = 7 − 1 − i | Extracted Character | New String |
|---|---|---|---|
| 0 | 6 | W | "W" |
| 1 | 5 | E | "WE" |
| 2 | 4 | I | "WEI" |
| 3 | 3 | V | "WEIV" |
| 4 | 2 | b | "WEIVb" |
| 5 | 1 | a | "WEIVba" |
| 6 | 0 | L | "WEIVbaL" |

**Result: `WEIVbaL`**

### Example 3: Input = `MADAM` (Palindrome)

- `String Length = 5`, indices run 4, 3, 2, 1, 0 and the characters extracted are M, A, D, A, M.

**Result: `MADAM`**, which is the same as the input, so the word is a palindrome.

### Example 4: Special cases

| Input | String Length | Loop Count | Result |
|---|---|---|---|
| (empty string) | 0 | 0 (loop does not run) | (empty string) |
| `A` | 1 | 1 (Index = 0) | `A` |
| `Hi 5` | 4 | 4 | `5 iH` (space is also reversed) |

---

## Test Cases and Results

| Input String | Expected Output |
|---|---|
| HELLO | OLLEH |
| LabVIEW | WEIVbaL |
| MADAM | MADAM |
| (empty) | (empty) |

---

## Learning Outcomes
- Understood how strings are stored as indexed sequences of characters.
- Used the String Length function to find the number of characters.
- Used a For Loop to process a string one character at a time.
- Derived and applied the index formula `Index = String Length − 1 − i` to read characters in reverse.
- Used String Subset to extract a single character.
- Used a Shift Register with Concatenate Strings to build a string across iterations.

---

## Tools Used
- LabVIEW
- For Loop
- Shift Register
- String Length function
- Subtract function
- String Subset function
- Concatenate Strings function
- String Control and String Indicator

---

## Conclusion
Successfully developed a LabVIEW VI that reverses an input string by reading its characters from the last position to the first with a For Loop, and building the reversed string through a shift register and the Concatenate Strings function.
