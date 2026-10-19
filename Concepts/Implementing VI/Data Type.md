# LabVIEW Data Types
A data type tells LabVIEW what kind of information a wire or terminal carries.

## 1. Numeric data types
LabVIEW has two common numeric categories:

<img width="1072" height="539" alt="image" src="https://github.com/user-attachments/assets/eca5da02-9c83-4e74-9fb7-0c5d60e154ab" />


Integer: Whole numbers without decimal points, such as 5, -12, and 100.

Floating-point: Numbers that can contain decimal values, such as 3.14, 0.25, and -7.5.

<img width="1108" height="397" alt="image" src="https://github.com/user-attachments/assets/9f438465-de74-4ed9-a509-d93d96dc73b1" />

For your Even/Odd Checker, we used I32 because we were working with whole numbers.
The numeric data type represents numbers of various types. 
**To change the representation type of a number, right-click the control, indicator, or constant, and select Representation.**
**How to change representation**
1. Right-click a Numeric Control, Indicator, or Constant.
2. Select Representation.
3. Choose I8, I16, I32, DBL, or another type.

## 2. Boolean data type
Boolean values have only two possibilities:

TRUE or FALSE.

For example:

Temperature > 50?

If the temperature is 60, the output is TRUE.

## 3. String data type
Strings contain text.

Examples:

"Hello", "EVEN", "Sensor disconnected".
4. Arrays
An array stores multiple elements of the same data type.

For example:

[10, 20, 30, 40]

This is a one-dimensional numeric array.

Arrays are very important in DSP because a sampled signal is often represented as an array of numeric values.

## 5. Clusters
A cluster groups different data types into one structure.

For example, an error cluster contains:
- Status — Boolean
- Code — Integer
- Source — String
Important: A broken wire can occur when you connect incompatible data types.

**LabVIEW Core 1 — Numeric Data Types Explained**
This section of Lesson 3 is about how LabVIEW stores numbers, how much memory they use, and what happens when you connect different numeric types together.

The most important concepts are:
1. Floating-point numbers
2. Fixed-point numbers
3. Signed and unsigned integers
4. Complex numbers
5. Numeric conversion and coercion dots
  
**1. What is numeric representation?**
   
Suppose you enter the number 25 in LabVIEW.

LabVIEW can store that number in different formats, such as:
- I32 — 32-bit signed integer
- U8 — 8-bit unsigned integer
- DBL — 64-bit floating-point number
The value may be the same, but the way LabVIEW stores it is different.

**How to change representation**
1. Right-click a Numeric Control, Indicator, or Constant.
2. Select Representation.
3. Choose I8, I16, I32, DBL, or another type.
In LabVIEW, orange wires usually represent floating-point numbers, while blue wires represent integers.

**2. Floating-Point Numbers**
Floating-point numbers are used for values that can contain decimal fractions.

Examples:

3.14, 0.005, -12.75, 100.0

<img width="1262" height="245" alt="image" src="https://github.com/user-attachments/assets/27723818-6bd2-4e24-acfd-ece484d68e9e" />

Example
Suppose you're measuring a voltage:
\[
V=2.5678912345
\]

An SGL value may retain only about 7 significant decimal digits.

A DBL value can retain approximately 15–16 significant decimal digits.

Which should you use?

For most beginner LabVIEW applications, DBL is a good default because it provides a useful balance of range and precision.

SGL can save memory when storing very large datasets.

EXT is generally reserved for situations where extra precision is genuinely needed.

Important: Floating-point values cannot represent every decimal fraction exactly. For example, 0.1 is usually stored as an approximation in binary floating-point.

**3. Fixed-Point Numbers**

This concept can initially seem confusing, so let's use a simple example.

Imagine you have a number:
\[
5.25
\]

In binary, it can be written as:
\[
101.01_2
\]

The digits after the binary point represent fractional values:
\[
101.01_2=4+1+0.25=5.25
\]

With fixed-point representation, you decide how many bits are available for the number and where the binary point is located.

Unlike floating-point, the scaling is fixed.

Three important properties

**Encoding**: Determines whether the number is signed or unsigned.

**Word length**: Total number of bits used to represent the number.

**Integer word length**: Determines the scaling and position of the binary point.

For a straightforward unsigned example with an 8-bit word length and 4 integer bits:
Example: 8-bit fixed-point number
<img width="1151" height="496" alt="image" src="https://github.com/user-attachments/assets/48a0db21-87f6-43e1-97d1-22e919a2bf87" />


Where is fixed-point used?
Fixed-point is especially useful in FPGA and embedded DSP systems, where memory, hardware resources, and processing capabilities may be limited.

For example, an FPGA-based digital filter might use fixed-point arithmetic instead of floating-point arithmetic.

**4. Integers**

Integers are whole numbers without fractional parts.

Examples:

0, 12, -25, 1000

In LabVIEW, integer wires are typically blue.

There are two main categories.

**Signed integers**

Signed integers can represent both positive and negative values.
<img width="899" height="293" alt="image" src="https://github.com/user-attachments/assets/d434a309-3055-4a0a-85dc-c5989c1e8d42" />

**Unsigned integers**

Unsigned integers represent only zero and positive values.
<img width="806" height="269" alt="image" src="https://github.com/user-attachments/assets/7bb3ad42-9d4f-4e5a-946c-8c1baae3e711" />

Why does U8 store larger positive values than I8?
Both use 8 bits.

An I8 uses its bit patterns to represent both negative and positive numbers.

A U8 uses all its bit patterns for nonnegative numbers.

When should you use integers?
Use integers for things like:
- Loop counters
- Array indices
- Number of samples
- Even/odd checking
For your Even/Odd Checker, I32 was an appropriate choice.
Important: Converting floating-point to integer
**LabVIEW's numeric conversion to an integer rounds to the nearest integer, using round-to-nearest-even when the value is exactly halfway.**

Examples:
<img width="1260" height="407" alt="image" src="https://github.com/user-attachments/assets/54ef5f1a-0578-4ced-9ded-686c52ed6305" />

**5. Complex Numbers**
Complex numbers contain a real part and an imaginary part.

They are written as:
\[
z=a+bi
\]

where \(i^2=-1\).

For example:
\[
z=3+4i
\]

Here:
- Real part = 3
- Imaginary part = 4
LabVIEW supports three common complex representations:
<img width="661" height="250" alt="image" src="https://github.com/user-attachments/assets/f1341352-44d3-4cdd-8716-8e0e70527001" />

Complex numeric wires are also orange.
Why are complex numbers important in DSP?
Complex numbers are widely used in:
- Fourier transforms (FFT)
- Frequency-domain analysis
- Digital modulation
- Signal phase calculations
- 
For example, an FFT can produce complex values representing frequency components, with magnitude and phase information.
  
  **6. Numeric Conversion and Coercion Dots**

This is one of the most important concepts in your manual.

Imagine you connect two different numeric types to a Multiply function:

- Input A = DBL (orange wire)
- Input B = I32 (blue wire)
LabVIEW needs compatible types to perform the multiplication.

It will usually convert the I32 input to DBL automatically.

This automatic conversion is called coercion.
Conceptual Multiply function: I32 is converted to DBL at the coercion dot.
<img width="1167" height="511" alt="image" src="https://github.com/user-attachments/assets/69887ffc-56c0-42bb-9927-bbcb4407a5e3" />

## What does the red coercion dot mean?

The small red dot on a function's input terminal indicates that LabVIEW is automatically converting the connected data to another representation.

## Boolean Values and Mechanical Actions in LabVIEW
This is an important topic in LabVIEW Core 1 — Lesson 3 because it explains how buttons, switches, and Stop controls behave.

The main thing to understand is the difference between Switch and Latch.

**1. What is a Boolean value?**

A Boolean has only two possible values:
- TRUE — ON, YES, or 1
- FALSE — OFF, NO, or 0
In LabVIEW, Boolean data is represented by green wires.

According to your manual, LabVIEW stores Boolean values using 8 bits:
- 00000000 → FALSE
- Any nonzero 8-bit value → TRUE
You can see Boolean controls as push buttons, toggle switches, and LEDs.

**2. What is Mechanical Action?**

Mechanical Action determines how a Boolean control changes when you click or release it, and whether it automatically returns to its default value.

There are six mechanical actions, divided into two groups:

- **Switch actions**: The control changes based on mouse interaction. Reading its value does not reset it.
- **Latch actions**: The control automatically resets to its default after LabVIEW reads the changed value, according to the selected latch behavior.
Let's understand them using a button that starts at FALSE.

**3. The three Switch actions**

**A. Switch When Pressed**

The value changes immediately when you press the mouse button.

For example:
- Initially FALSE
- Click → TRUE
- Click again → FALSE
It stays in its new state until you click again.

Real-life example: A light switch.

**B. Switch When Released**

The value changes only when you release the mouse button inside the control.
- Initially FALSE
- Press → Still FALSE
- Release → TRUE
- Press and release again → FALSE
Difference: Switch When Pressed changes on the press; Switch When Released changes on the release.

**C. Switch Until Released**

The control changes when pressed and returns to its original value when released.
- Initially FALSE
- Press and hold → TRUE
- Release → FALSE
Real-life example: A doorbell.

This is useful when you want an action to remain active only while a button is held.

**4. The three Latch actions**

The important thing about Latch actions is that LabVIEW must read the control's value for the latch to reset.

**D. Latch When Pressed**

The control changes when you press it.

It stays changed until LabVIEW reads the value once, then resets to its default.

Example:
- Initially FALSE
- Press → TRUE
- VI reads TRUE → Automatically returns to FALSE
Even if you are still holding the mouse button, the control can reset once it is read.

Common use: A Stop Button for a While Loop.

**E. Latch When Released**

The control changes only when you release the mouse button inside it.

Then it waits until LabVIEW reads the value and resets.

Example:
- Initially FALSE
- Press → Still FALSE
- Release → TRUE
- VI reads TRUE → Returns to FALSE
Real-life example: An OK button in a dialog box.

**F. Latch Until Released**

The control changes when pressed.

It returns to its default only after both conditions have happened:
1. You release the mouse button.
2. LabVIEW reads the changed value.
Whichever happens last determines when it resets.

For example, if LabVIEW reads the value while you're still holding the button, it remains changed until you release it.
**Comparison table — all six actions**
<img width="1091" height="473" alt="image" src="https://github.com/user-attachments/assets/b55ef317-62aa-43ed-a8e8-da79649f9800" />

**How to change Mechanical Action in LabVIEW**

1. Open the Front Panel.
2. Place a Boolean control, such as a Push Button or Stop Button.
3. Right-click the Boolean control.
4. Select Mechanical Action.
5. Choose one of the six options.
Note: Some Boolean controls, such as radio buttons, do not support all six actions.

**Which action should you use for a Stop Button?**

Suppose you have a While Loop that continuously generates random numbers.

You want the loop to stop when you press STOP.

A common choice is Latch When Released, which is the usual default behavior for LabVIEW's Stop Button.

Why?
1. You click STOP.
2. LabVIEW changes the Boolean to TRUE when you release the mouse.
3. The While Loop reads TRUE and stops.
4. The Boolean automatically resets to FALSE.
This makes the button ready for the next run.

Important: A latching Boolean should be read through its terminal in the appropriate execution path. Local variables cannot be used to read a latching Boolean's value.

## Strings, Enums, and Dynamic Data Types
This section of Lesson 3: Implementing a VI introduces three important LabVIEW data types:
1. Strings — Store text and characters.
2. Enums — Let the user select an option from a predefined list.
3. Dynamic Data — Commonly used with Express VIs for signals and measurements.

**1. Strings in LabVIEW**
A string is a sequence of characters, such as letters, numbers, spaces, and special characters.
<img width="1035" height="554" alt="image" src="https://github.com/user-attachments/assets/c4ca41cc-35d7-4c84-aa5a-9263d236e5fb" />

**2.Enums (Enumerated Data Types)**
An Enum is a control that lets you choose one value from a predefined list of named options.

Imagine you want the user to select a month.

Instead of asking them to type the month manually, you provide a list:

January, February, March, ... December

Each item has a corresponding numeric value.

<img width="1261" height="433" alt="image" src="https://github.com/user-attachments/assets/1c2f2d7e-27d8-495e-ada1-55a82e7c695a" />

So an Enum combines a readable name with an underlying numeric value.

**Why are Enums useful?**

Imagine you're creating a motor controller with three modes:
- OFF
- FORWARD
- REVERSE
An Enum allows the user to select the mode by name.

On the Block Diagram, you can wire the Enum to a Case Structure to execute the appropriate operation.

**How to create an Enum in LabVIEW**

1. Open the Front Panel.
2. Right-click → Ring & Enum → Enum.
3. Place the Enum control.
4. Right-click it and select Edit Items (or open its Properties and go to Edit Items).
5. Add your options, such as OFF, FORWARD, and REVERSE.
6. Click OK.
You can then wire the Enum to a Case Structure.

Remember: Enums are particularly useful when you want readable choices but also want LabVIEW to work with their underlying numeric values.

**3.Dynamic Data Type**
Dynamic Data is a special LabVIEW data type commonly produced by Express VIs.

For example, you might use a Simulate Signal Express VI to generate a sine wave.

The Express VI can output dynamic data containing signal information.

Dynamic data wires are usually dark blue and thicker than ordinary numeric wires.

**What if another function doesn't accept Dynamic Data?**

Many standard LabVIEW functions expect numeric arrays or waveforms rather than dynamic data.

In that situation, use:

**How to convert Dynamic Data**
1. Open the Block Diagram.
2. Place Convert from Dynamic Data from Express → Signal Manipulation.
3. Connect the dynamic data wire to its input.
4. Double-click the conversion VI to configure the output type.
5. Choose the appropriate representation, such as a numeric array or waveform.
LabVIEW can also insert this conversion automatically in certain situations, such as wiring dynamic data to an array indicator.
