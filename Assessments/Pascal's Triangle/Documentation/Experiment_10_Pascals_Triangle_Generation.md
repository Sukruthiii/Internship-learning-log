# Experiment 10 – Pascal's Triangle Generation

## Aim
To create a VI that generates Pascal's Triangle up to a specified number of rows and displays the result using a 2D Numeric Array Indicator.

---

## Theory

### What is Pascal's Triangle?
**Pascal's Triangle** is a triangular arrangement of numbers in which:

- The first row is `1`.
- Every row starts and ends with `1`.
- Every other number is the **sum of the two numbers directly above it** in the previous row.

```text
Row 0:            1
Row 1:          1   1
Row 2:        1   2   1
Row 3:      1   3   3   1
Row 4:    1   4   6   4   1
```

### Link with combinations (binomial coefficients)
The number at row `n`, position `k` (both counted from 0) is the binomial coefficient `C(n, k)`:

```text
C(n, k) = n! / (k! × (n − k)!)
```

and Pascal's rule is:

```text
C(n, k) = C(n − 1, k − 1) + C(n − 1, k)
```

Example: `C(4, 2) = 4! / (2! × 2!) = 24 / 4 = 6`, which matches the middle number of row 4.

### Useful properties
- Row `n` contains `n + 1` numbers.
- The sum of the numbers in row `n` is `2ⁿ` (row 4: 1 + 4 + 6 + 4 + 1 = 16 = 2⁴).
- Each row is symmetric.
- Row `n` gives the coefficients of `(a + b)ⁿ`, e.g. `(a + b)² = 1a² + 2ab + 1b²`.

### Generating a row from the previous row
Place the previous row in an array and add to it a copy of itself **shifted one position to the right**:

```text
Previous row :  1  3  3  1  0
Shifted copy :  0  1  3  3  1
Sum          :  1  4  6  4  1   (next row)
```

Each element of the new row is the sum of the element above it and the element to the left of that one, which is exactly Pascal's rule:

```text
New[k] = Previous[k] + Previous[k − 1]
```

### Rotate 1D Array
The **Rotate 1D Array** function moves the elements of an array by a given number of positions, with the element that moves off one end re-entering at the other end. With a rotation of 1, every element moves one position to the right and the last element moves to the front:

```text
[a, b, c, d, e]  →  [e, a, b, c, d]
```

Because the arrays are padded with zeros, the last element of each generated row is 0, so the wrapped-around value does not disturb the rows that are collected for output.

### Shift register and auto-indexing
- A **shift register** carries the previous row from one iteration to the next.
- An **auto-indexing output tunnel** on a For Loop collects the value wired to it in every iteration into an array. Collecting 1D rows this way builds a **2D array** (one row per iteration).

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Initial row | Array of `R` zeros, with element 0 replaced by 1 |
| Loop count | `N = Number of Rows (R)` |
| Rotated row | `Rotated[k] = Previous[k − 1]`, and `Rotated[0] = Previous[R − 1]` |
| Next row | `Next = Previous + Rotated` (element by element) |
| Collected row | The **previous** row of each iteration (left shift register) |
| Output size | `R × R` 2D array (rows padded with zeros) |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Number of Rows | Numeric Control | Specifies the number of rows to generate |
| Pascal's Triangle | 2D Numeric Array Indicator | Displays the generated triangle |

---

## Block Diagram Implementation

1. **Initialize Array:** Creates an array of zeros with the size specified by the Number of Rows input.
2. **Replace Array Subset:** Replaces the first element of the initialized array with **1** to form the initial row.
3. **For Loop:** Repeats the row-generation process for the specified number of iterations.
4. **Shift Register:** Stores the previous row and passes it to the next iteration. The previous row is also connected to an auto-indexing output tunnel to collect all generated rows.
5. **Rotate 1D Array:** Rotates the previous row by one position.
6. **Add Function:** Adds the previous row and the rotated array element by element to generate the next row.
7. The resulting array is fed back to the shift register for the next iteration.
8. The left shift register is connected to the auto-indexing output tunnel to collect the previous row during each iteration. This ensures that the initial row is also included in the final 2D array, while the right shift register stores the newly generated row for the next iteration.

---

## Worked Examples

### Example 1: Number of Rows = 5

- Initialize Array: `[0, 0, 0, 0, 0]`
- Replace Array Subset (element 0 = 1): `[1, 0, 0, 0, 0]`
- Loop count `N = 5`

| Iteration | Previous row (collected) | Rotated row | Next row = Previous + Rotated (to shift register) |
|---|---|---|---|
| 1 | [1, 0, 0, 0, 0] | [0, 1, 0, 0, 0] | [1, 1, 0, 0, 0] |
| 2 | [1, 1, 0, 0, 0] | [0, 1, 1, 0, 0] | [1, 2, 1, 0, 0] |
| 3 | [1, 2, 1, 0, 0] | [0, 1, 2, 1, 0] | [1, 3, 3, 1, 0] |
| 4 | [1, 3, 3, 1, 0] | [0, 1, 3, 3, 1] | [1, 4, 6, 4, 1] |
| 5 | [1, 4, 6, 4, 1] | [1, 1, 4, 6, 4] | (not collected) |

The auto-indexing tunnel collects the **Previous row** of every iteration, so the output is:

| | Col 0 | Col 1 | Col 2 | Col 3 | Col 4 |
|---|---|---|---|---|---|
| Row 0 | 1 | 0 | 0 | 0 | 0 |
| Row 1 | 1 | 1 | 0 | 0 | 0 |
| Row 2 | 1 | 2 | 1 | 0 | 0 |
| Row 3 | 1 | 3 | 3 | 1 | 0 |
| Row 4 | 1 | 4 | 6 | 4 | 1 |

The non-zero entries form Pascal's Triangle. Check: row 4 sums to `1 + 4 + 6 + 4 + 1 = 16 = 2⁴`.

### Example 2: Number of Rows = 3

- Initial row: `[1, 0, 0]`

| Iteration | Previous row (collected) | Rotated row | Next row |
|---|---|---|---|
| 1 | [1, 0, 0] | [0, 1, 0] | [1, 1, 0] |
| 2 | [1, 1, 0] | [0, 1, 1] | [1, 2, 1] |
| 3 | [1, 2, 1] | [1, 1, 2] | (not collected) |

**Output (3 × 3):**

```text
1  0  0
1  1  0
1  2  1
```

### Example 3: Number of Rows = 1

- Initial row: `[1]`
- The loop runs once and collects the previous row `[1]`.

**Output:** `[[1]]`

---

## Test Cases and Results

| Number of Rows | Expected Output (2D Array) |
|---|---|
| 1 | [[1]] |
| 3 | [[1, 0, 0], [1, 1, 0], [1, 2, 1]] |
| 5 | [[1, 0, 0, 0, 0], [1, 1, 0, 0, 0], [1, 2, 1, 0, 0], [1, 3, 3, 1, 0], [1, 4, 6, 4, 1]] |

---

## Note
- Every row of the 2D array has the same length (equal to the number of rows), so the shorter rows are padded with zeros on the right.
- The Number of Rows should be at least 1.

---

## Learning Outcomes
- Understood the structure of Pascal's Triangle and its link with binomial coefficients.
- Used Initialize Array and Replace Array Subset to create the starting row.
- Used Rotate 1D Array and the Add function to build each new row from the previous one.
- Used a Shift Register to carry the current row between iterations.
- Used an auto-indexing output tunnel to collect 1D rows into a 2D array.

---

## Tools Used
- LabVIEW
- For Loop
- Shift Register
- Auto-indexing Output Tunnel
- Initialize Array function
- Replace Array Subset function
- Rotate 1D Array function
- Add function
- Numeric Control and 2D Numeric Array Indicator

---

## Conclusion
Successfully developed a LabVIEW VI that generates Pascal's Triangle for a specified number of rows by repeatedly adding each row to its rotated copy inside a For Loop, with a shift register carrying the current row and an auto-indexing tunnel collecting all rows into a 2D array.
