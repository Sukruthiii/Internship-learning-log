# Experiment 05 – News Bulletin Interface

## Aim
To create a VI that displays multiple news headlines and allows the user to navigate through them manually or view them automatically at fixed time intervals.

---

## Theory

### Arrays in LabVIEW
An **array** is an ordered collection of elements of the same data type. In this VI, a **1D array of strings** holds the news headlines.

- Each element is identified by an **index**.
- **LabVIEW indexing starts from 0**, so the first headline is at index 0 and the last is at index `Total − 1`.
- **Array Size** returns the number of elements in the array.
- **Index Array** returns the element stored at a given index.

### Shift Register
A **shift register** carries a value from one iteration of a loop to the next. It has a left terminal (value from the previous iteration) and a right terminal (value to pass to the next iteration). Here it stores the **current headline index**, and it is initialized to 0 so the VI starts at the first headline.

### Modulo operation and circular navigation
The **Quotient & Remainder** function returns the remainder of a division. Using it with the total number of headlines makes the index **wrap around**:

```text
Next Index = (Current Index + 1) mod Total Headlines
```

When the index reaches the last headline, the next value becomes 0 again, so the headlines repeat in a loop like a real news ticker.

### Manual and Automatic modes
| Mode | How the headline changes | Wait time |
|---|---|---|
| Manual | Index advances only when the **Next** button is pressed | 20 ms (prevents excessive CPU usage) |
| Automatic | Index advances on every loop iteration | 1000 ms (one headline per second) |

The loop's **Wait (ms)** function sets how often each iteration runs. In Automatic mode the 1000 ms wait is what makes the headline change every second.

### Formulas used in the VI

| Quantity | Formula |
|---|---|
| Total Headlines | `Total = Array Size(Headlines)` |
| Current Headline | `Headlines[Index]` |
| Headline Number (display) | `Headline Number = Index + 1` |
| Next Index (Automatic) | `(Index + 1) mod Total` |
| Next Index (Manual) | `(Index + 1) mod Total` if Next = TRUE, else `Index` |
| Shift register initial value | `0` |

---

## Front Panel

| Element | Type | Purpose |
|---|---|---|
| Headlines | String Array Control | Enter multiple news headlines |
| Total Headlines | Numeric Indicator | Displays the total number of headlines |
| Headline Number | Numeric Indicator | Displays the current headline number (starting from 1) |
| Current Headline | String Indicator | Displays the current headline |
| Mode | Enum Control | Selects between Manual and Automatic modes |
| Next | Boolean Push Button | Moves to the next headline in Manual mode |
| Stop | Boolean Push Button | Stops the VI |

---

## Block Diagram Implementation

1. **While Loop:** A While Loop continuously executes the program until the Stop button is pressed.
2. **Array Size:** The *Array Size* function calculates the total number of headlines entered in the String Array and displays the count using the Total Headlines indicator.
3. **Index Array:** The *Index Array* function retrieves the headline corresponding to the current array index and displays it using the Current Headline indicator.
4. **Shift Register:** A Shift Register stores the current headline index between loop iterations. It is initialized to **0** because array indexing starts from 0 in LabVIEW.
5. **Headline Number:** An *Increment* function adds 1 to the current index to display the headline number in a user-friendly format starting from 1.
6. **Case Structure:** The Mode Enum control is connected to the Case Structure selector to implement Manual and Automatic modes.
7. **Manual Mode:** A *Select* function checks the state of the Next button.
   - **TRUE:** The index advances to the next headline.
   - **FALSE:** The current index is retained.
   - A *Wait (ms)* function with a **20 ms** delay prevents excessive CPU usage.
8. **Automatic Mode:** The index automatically advances to the next headline. A *Wait (ms)* function with a **1000 ms** delay ensures that the headline changes every one second.
9. **Index Calculation:** An *Increment* function adds 1 to the current index. The *Quotient & Remainder* function performs the modulo operation using the total number of headlines, ensuring that the index returns to 0 after reaching the last headline.
10. **Result Storage:** The output of the Case Structure is connected to the right Shift Register terminal, which stores the updated index for the next loop iteration.
11. **Stop Button:** The Stop button is connected to the conditional terminal of the While Loop to terminate the VI when pressed.

---

## Worked Examples

Consider a Headlines array with **4 headlines**:

| Index | Headline |
|---|---|
| 0 | Headline A |
| 1 | Headline B |
| 2 | Headline C |
| 3 | Headline D |

`Total Headlines = Array Size = 4`

### Example 1: Automatic Mode

The shift register starts at 0. In each iteration, the headline at the current index is displayed, then the next index is calculated as `(Index + 1) mod 4`, with a 1000 ms wait.

| Iteration | Index | Headline Number (Index + 1) | Current Headline | Next Index = (Index + 1) mod 4 | Time |
|---|---|---|---|---|---|
| 1 | 0 | 1 | Headline A | (0 + 1) mod 4 = 1 | 0 s |
| 2 | 1 | 2 | Headline B | (1 + 1) mod 4 = 2 | 1 s |
| 3 | 2 | 3 | Headline C | (2 + 1) mod 4 = 3 | 2 s |
| 4 | 3 | 4 | Headline D | (3 + 1) mod 4 = **0** | 3 s |
| 5 | 0 | 1 | Headline A | (0 + 1) mod 4 = 1 | 4 s |

After the last headline (index 3), the modulo operation brings the index back to 0, so the headlines repeat continuously until Stop is pressed.

### Example 2: Manual Mode

The index changes only when Next is TRUE. Each iteration waits 20 ms.

| Iteration | Next Button | Select Output | Index | Current Headline |
|---|---|---|---|---|
| 1 | FALSE | Index retained (0) | 0 | Headline A |
| 2 | FALSE | Index retained (0) | 0 | Headline A |
| 3 | TRUE | (0 + 1) mod 4 = 1 | 1 | Headline B |
| 4 | FALSE | Index retained (1) | 1 | Headline B |
| 5 | TRUE | (1 + 1) mod 4 = 2 | 2 | Headline C |

### Example 3: Wrap-around from the last headline (Manual Mode)

If the current index is 3 (Headline D) and Next is pressed:

```text
Next Index = (3 + 1) mod 4 = 4 mod 4 = 0
```

The display returns to Headline A (Headline Number 1).

---

## Test Cases and Results

| Mode | Action | Expected Output |
|---|---|---|
| Manual | Run VI, do not press Next | Headline 1 stays displayed |
| Manual | Press Next once | Headline changes to the next one (Headline Number increases by 1) |
| Manual | Press Next on the last headline | Returns to Headline 1 |
| Automatic | Run VI | Headline changes every 1 second |
| Automatic | Let it run past the last headline | Wraps back to Headline 1 |
| Any | Press Stop | VI stops |

---

## Note
At least one headline must be entered in the array. If the array is empty, `Total Headlines = 0` and the modulo operation by 0 has no valid result.

---

## Learning Outcomes
- Learned to use a String Array Control to store multiple text values.
- Used Array Size and Index Array to count and retrieve array elements.
- Used a Shift Register to store a value between loop iterations.
- Implemented circular navigation using the Quotient & Remainder (modulo) function.
- Used an Enum Control with a Case Structure to switch between Manual and Automatic modes.
- Controlled loop timing using the Wait (ms) function.
- Used a Boolean Stop button to terminate a While Loop.

---

## Tools Used
- LabVIEW
- While Loop
- Shift Register
- Case Structure
- Array Size function
- Index Array function
- Increment function
- Select function
- Quotient & Remainder function
- Wait (ms) function
- String Array Control, Enum Control, Boolean Push Buttons
- Numeric and String Indicators

---

## Conclusion
Successfully developed a LabVIEW VI that displays news headlines from a string array, with manual navigation using a Next button and automatic navigation at one-second intervals, using a shift register and modulo operation to cycle through the headlines continuously.
