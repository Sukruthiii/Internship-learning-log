# LabVIEW Data Types
A data type tells LabVIEW what kind of information a wire or terminal carries.

## 1. Numeric data types
LabVIEW has two common numeric categories:

<img width="1072" height="539" alt="image" src="https://github.com/user-attachments/assets/eca5da02-9c83-4e74-9fb7-0c5d60e154ab" />


Integer: Whole numbers without decimal points, such as 5, -12, and 100.

Floating-point: Numbers that can contain decimal values, such as 3.14, 0.25, and -7.5.

<img width="1108" height="397" alt="image" src="https://github.com/user-attachments/assets/9f438465-de74-4ed9-a509-d93d96dc73b1" />

For your Even/Odd Checker, we used I32 because we were working with whole numbers.

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
