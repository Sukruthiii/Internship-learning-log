# Experiment 12 – Sort an Array

## Aim
To develop a LabVIEW VI that sorts an array of numeric elements in ascending or descending order using the Bubble Sort algorithm.

---

## Theory

### What is sorting?
**Sorting** is arranging the elements of a list in a specific order.

- **Ascending order:** smallest to largest, e.g. 1, 2, 5, 9
- **Descending order:** largest to smallest, e.g. 9, 5, 2, 1

### Arrays in LabVIEW
An **array** is an ordered collection of elements of the same data type, and each element is identified by an **index**. LabVIEW indexing starts from **0**, so an array of `N` elements has indices `0` to `N − 1`.

| Function | Purpose |
|---|---|
| Array Size | Returns the number of elements `N` |
| Index Array | Returns the element at a given index |
| Replace Array Subset | Replaces the element at a given index with a new value |

### Bubble Sort
**Bubble Sort** repeatedly compares **adjacent elements** and swaps them if they are in the wrong order. After each pass, the largest (for ascending) or smallest (for descending) remaining element "bubbles" to its final position at the end of the array.

| Order | Swap condition for adjacent elements `A[j]` and `A[j+1]` |
|---|---|
| Ascending | Swap if `A[j] > A[j+1]` (Greater?) |
| Descending | Swap if `A[j] < A[j+1]` (Less?) |

### Swapping two elements
To exchange two adjacent elements, each one is written into the other's position:

```text
A[j]   (new) = A[j+1] (old)
A[j+1] (new) = A[j]   (old)
```

In the VI, this is done with two **Replace Array Subset** functions.

### Number of passes and comparisons
For an array of `N` elements:

- The **outer loop** runs `N − 1` passes.
- In pass `i` (starting from `i = 0`), the **inner loop** runs `N − 1 − i` comparisons, because the last `i` elements are already in their final positions.
- The index `j` runs from `0` to `N − 2 − i`.

Total number of comparisons:

```text
(N − 1) + (N − 2) + ... + 1 = N(N − 1) / 2
```

For N = 4, this is 3 + 2 + 1 = 6 comparisons.

### Shift registers
- The **inner loop** shift register carries the updated array from one comparison to the next.
- The **outer loop** shift register carries the updated array from one pass to the next.

Both are initialized with the input array.

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Number of elements | `N = Array Size(Input Array)` |
| Outer loop count (passes) | `N − 1` |
| Inner loop count (pass i) | `N − 1 − i` |
| Adjacent elements compared | `A[j]` and `A[j+1]`, with `j = 0, 1, ..., N − 2 − i` |
| Ascending swap condition | `A[j] > A[j+1]` |
| Descending swap condition | `A[j] < A[j+1]` |
| Total comparisons | `N(N − 1) / 2` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Input Array | Numeric Array Control | Enter the input elements |
| Sorting Order | Enum Control | Select Ascending or Descending order |
| Sorted Array | Numeric Array Indicator | Displays the sorted result |

---

## Block Diagram Implementation

1. **Outer For Loop:** Controls the number of sorting passes. It executes `N − 1` times, where `N` is the number of elements in the array.
2. **Inner For Loop:** Compares adjacent elements during each pass. The number of comparisons decreases after each pass.
3. **Index Array:** Retrieves two adjacent elements using their respective indices (`j` and `j + 1`).
4. **Comparison Logic:** Uses *Greater?* for ascending order and *Less?* for descending order to determine whether the elements need to be swapped.
5. **Case Structures:**
   - The first Case Structure selects the comparison operation based on the Sorting Order Enum.
   - The second Case Structure performs swapping when the comparison result is TRUE and retains the original array when it is FALSE.
6. **Replace Array Subset:** Two *Replace Array Subset* functions are used to exchange the positions of adjacent elements when swapping is required.
7. **Shift Registers:** Two pairs of shift registers are used. The inner loop shift register carries the updated array between comparisons, while the outer loop shift register carries the updated array between sorting passes.

---

## Sorting Algorithm

Bubble Sort repeatedly compares adjacent elements and swaps them if they are in the wrong order.

- **Ascending Order:** If the first element is greater than the second element, they are swapped.
- **Descending Order:** If the first element is smaller than the second element, they are swapped.

The process continues until all elements are arranged in the selected order.

---

## Worked Examples

### Example 1: Ascending order, Input = [5, 2, 9, 1]

- `N = 4`, so the outer loop runs `N − 1 = 3` passes.

**Pass 1 (i = 0):** inner loop runs `4 − 1 − 0 = 3` comparisons

| j | Compare | Condition `A[j] > A[j+1]` | Action | Array after |
|---|---|---|---|---|
| 0 | 5 and 2 | 5 > 2 TRUE | Swap | [2, 5, 9, 1] |
| 1 | 5 and 9 | 5 > 9 FALSE | Keep | [2, 5, 9, 1] |
| 2 | 9 and 1 | 9 > 1 TRUE | Swap | [2, 5, 1, 9] |

The largest element, 9, is now at the end.

**Pass 2 (i = 1):** inner loop runs `4 − 1 − 1 = 2` comparisons

| j | Compare | Condition `A[j] > A[j+1]` | Action | Array after |
|---|---|---|---|---|
| 0 | 2 and 5 | 2 > 5 FALSE | Keep | [2, 5, 1, 9] |
| 1 | 5 and 1 | 5 > 1 TRUE | Swap | [2, 1, 5, 9] |

**Pass 3 (i = 2):** inner loop runs `4 − 1 − 2 = 1` comparison

| j | Compare | Condition `A[j] > A[j+1]` | Action | Array after |
|---|---|---|---|---|
| 0 | 2 and 1 | 2 > 1 TRUE | Swap | [1, 2, 5, 9] |

**Result: [1, 2, 5, 9]**

### Example 2: Descending order, Input = [5, 2, 9, 1]

Swap condition is `A[j] < A[j+1]` (Less?).

| Pass | j | Compare | Condition `A[j] < A[j+1]` | Action | Array after |
|---|---|---|---|---|---|
| 1 | 0 | 5 and 2 | 5 < 2 FALSE | Keep | [5, 2, 9, 1] |
| 1 | 1 | 2 and 9 | 2 < 9 TRUE | Swap | [5, 9, 2, 1] |
| 1 | 2 | 2 and 1 | 2 < 1 FALSE | Keep | [5, 9, 2, 1] |
| 2 | 0 | 5 and 9 | 5 < 9 TRUE | Swap | [9, 5, 2, 1] |
| 2 | 1 | 5 and 2 | 5 < 2 FALSE | Keep | [9, 5, 2, 1] |
| 3 | 0 | 9 and 5 | 9 < 5 FALSE | Keep | [9, 5, 2, 1] |

**Result: [9, 5, 2, 1]**

### Example 3: Duplicate values, Ascending, Input = [3, 3, 1]

- `N = 3`, so there are 2 passes.

| Pass | j | Compare | Condition `A[j] > A[j+1]` | Action | Array after |
|---|---|---|---|---|---|
| 1 | 0 | 3 and 3 | 3 > 3 FALSE | Keep | [3, 3, 1] |
| 1 | 1 | 3 and 1 | 3 > 1 TRUE | Swap | [3, 1, 3] |
| 2 | 0 | 3 and 1 | 3 > 1 TRUE | Swap | [1, 3, 3] |

**Result: [1, 3, 3]** (equal elements are not swapped, since the comparison is strictly greater or less)

### Example 4: Special cases

| Input | Order | Behaviour | Result |
|---|---|---|---|
| [1, 2, 3] (already sorted) | Ascending | No swaps happen in any comparison | [1, 2, 3] |
| [7] (single element) | Either | `N − 1 = 0`, so the loops do not run | [7] |

---

## Test Cases and Results

| Input Array | Sorting Order | Expected Output |
|---|---|---|
| [5, 2, 9, 1] | Ascending | [1, 2, 5, 9] |
| [5, 2, 9, 1] | Descending | [9, 5, 2, 1] |
| [3, 3, 1] | Ascending | [1, 3, 3] |
| [1, 2, 3] | Ascending | [1, 2, 3] |

---

## Learning Outcomes
- Understood the Bubble Sort algorithm and how adjacent comparisons move elements into position.
- Used nested For Loops, with the inner loop count decreasing after each pass.
- Used Index Array to read adjacent elements and Replace Array Subset to swap them.
- Used an Enum Control with a Case Structure to select ascending or descending comparison.
- Used shift registers on both loops to carry the updated array between comparisons and passes.

---

## Tools Used
- LabVIEW
- Outer and Inner For Loops
- Shift Registers
- Case Structures
- Index Array function
- Replace Array Subset function
- Greater? and Less? functions
- Array Size function
- Numeric Array Control and Indicator, Enum Control

---

## Conclusion
Successfully developed a LabVIEW VI that sorts a numeric array in ascending or descending order using the Bubble Sort algorithm, with nested For Loops, comparison and swap logic in Case Structures, and shift registers carrying the updated array.
