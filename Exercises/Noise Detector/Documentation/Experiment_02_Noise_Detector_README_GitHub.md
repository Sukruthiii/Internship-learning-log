# Experiment 2 -- Noise Detector

**Platform:** LabVIEW\
**Domain:** Digital Signal Processing (DSP)\
**Experiment:** Generation of a Noisy Signal and Noise Level Detection

------------------------------------------------------------------------

## 1. Objective

To generate a sine wave and introduce random noise into it using
LabVIEW. Calculate the average absolute noise level and determine
whether the noise exceeds a predefined threshold.

This experiment demonstrates basic DSP concepts such as signal
generation, random noise generation, signal addition, array operations,
statistical analysis, and threshold-based noise detection.

## 2. Experiment Overview

In practical applications, signals acquired from sensors and electronic
systems may contain unwanted noise.

In this experiment, a clean sine wave is generated and random noise is
added to simulate a noisy signal.

The VI performs the following operations:

1.  Generate an original sine wave.
2.  Generate random noise.
3.  Add random noise to the original signal.
4.  Display both signals on the same Waveform Graph.
5.  Calculate the average absolute noise level.
6.  Compare the calculated noise level with a predefined threshold.
7.  Display the noise status as NORMAL or NOISY.

## 3. Front Panel Design

The Front Panel is organized using two clusters.

### 3.1 Signal Configuration Cluster (Controls)

  Control                   Description
  ------------------------- -------------------------------------------------
  Signal Frequency (Hz)     Frequency of the original sine wave
  Sampling Frequency (Hz)   Number of samples generated per second
  Number of Samples         Total number of samples generated
  Noise Amplitude           Controls the strength of random noise
  Noise Threshold           Maximum acceptable average absolute noise level

### 3.2 Noise Analysis Results Cluster (Indicators)

  -----------------------------------------------------------------------
  Indicator                           Description
  ----------------------------------- -----------------------------------
  Signal Comparison                   Displays original and noisy signals
                                      on the same Waveform Graph

  Average Noise Level                 Displays the calculated average
                                      absolute noise

  Noise Status                        Displays NORMAL or NOISY based on
                                      the threshold comparison
  -----------------------------------------------------------------------

**Why use Clusters?**

Clusters allow different data types to be grouped together as a single
data structure. They help organize related controls and indicators and
make the Block Diagram easier to manage.

`Unbundle by Name` is used to extract individual controls from the input
cluster.

## 4. Mathematical Concepts and Formulas

### 4.1 Original Sine-Wave Generation

The original sine wave is generated using:

$$
s$$n$$=`\sin`{=tex}`\left`{=tex}(`\frac{2\pi fn}{F_s}`{=tex}`\right`{=tex})
$$

Where:

-   `s[n]` = Original sine signal
-   `f` = Signal frequency
-   `n` = Sample index
-   `Fs` = Sampling frequency

**Example values:**

-   Signal Frequency = 10 Hz
-   Sampling Frequency = 1000 Hz
-   Number of Samples = 1000

The sampling interval is:

$$ T_s=`\frac{1}{F_s}`{=tex} $$

$$ T_s=`\frac{1}{1000}`{=tex}=0.001`\text{ seconds}`{=tex} $$

This means one sample is generated every 1 millisecond.

The For Loop executes 1000 times, generating one sine-wave sample during
each iteration.

### 4.2 Random Noise Generation

The Random Number (0--1) function generates random values between 0 and
1.

To generate both positive and negative noise values, the following
equation is used:

$$ v$$n$$=A(2r$$n$$-1) $$

Where:

-   `v[n]` = Noise sample
-   `A` = Noise amplitude
-   `r[n]` = Random number between 0 and 1

**Why multiply by 2?**

Multiplying the random number by 2 changes its range from 0--1 to 0--2.

**Why subtract 1?**

Subtracting 1 shifts the range from 0--2 to -1--+1.

**Why multiply by Noise Amplitude?**

Noise amplitude controls the strength of the generated noise.

For example, if A = 0.5:

$$ -0.5`\leq `{=tex}v$$n$$`\leq 0.5`{=tex} $$

Therefore, the generated noise contains both positive and negative
values.

### 4.3 Noisy Signal Generation

The original sine signal and random noise are combined using:

$$ x$$n$$=s$$n$$+v$$n$$ $$

Where:

-   `x[n]` = Noisy signal
-   `s[n]` = Original sine signal
-   `v[n]` = Random noise

The Add function performs element-wise addition of the two arrays.

For example:

    Original Signal   Noise   Noisy Signal
  ----------------- ------- --------------
                0.2     0.1            0.3
                0.5    -0.2            0.3
                0.8     0.3            1.1
               -0.4     0.1           -0.3

### 4.4 Average Absolute Noise Calculation

The average absolute noise level is calculated using:

$$
N\_{`\text{avg}`{=tex}}=`\frac{1}{N}`{=tex}`\sum`{=tex}\_{n=0}\^{N-1}\|v$$n$$\|
$$

Where:

-   `N_avg` = Average absolute noise level
-   `N` = Total number of noise samples
-   `v[n]` = Individual noise sample
-   `|v[n]|` = Absolute value of the noise sample

**Why use Absolute Value (Abs)?**

Consider the following noise samples:

$$ $$-0.2, 0.4, -0.1, 0.3$$ $$

If we calculate the ordinary average:

$$ `\frac{-0.2+0.4-0.1+0.3}{4}`{=tex}=0.1 $$

Positive and negative values partially cancel each other.

However, we want to measure the magnitude of noise irrespective of its
direction.

Therefore, we apply the Absolute Value function:

$$ $$0.2, 0.4, 0.1, 0.3$$ $$

Then calculate the average:

$$ N\_{`\text{avg}`{=tex}}=`\frac{0.2+0.4+0.1+0.3}{4}`{=tex} $$

$$ `\boxed{N_{\text{avg}}=0.25}`{=tex} $$

**Why do we expect approximately 0.25 in our experiment?**

Our noise is uniformly distributed between -A and +A.

The theoretical expected average absolute noise is:

$$ E$$\|v$$n$$\|$$=`\frac{A}{2}`{=tex} $$

For A = 0.5:

$$ E$$\|v$$n$$\|$$=`\frac{0.5}{2}`{=tex} $$

$$ `\boxed{E[|v[n]|]=0.25}`{=tex} $$

This is a theoretical expected value, not a guaranteed result. The
actual output varies because the noise samples are randomly generated.

### 4.5 Noise Detection

The calculated average absolute noise level is compared with the
user-defined threshold.

$$ N\_{`\text{avg}`{=tex}}\>N\_{`\text{threshold}`{=tex}} $$

The detection logic is:

-   If Average Noise Level \> Threshold: **NOISY**
-   If Average Noise Level \<= Threshold: **NORMAL**

A Greater? comparison function produces a Boolean output (TRUE or
FALSE), which controls a Case Structure.

The Case Structure displays the corresponding string in the Noise Status
indicator.

------------------------------------------------------------------------

## 5. Block Diagram Implementation

### Step 1: Extracting Cluster Elements

The Unbundle by Name function is used to extract individual controls
from the Signal Configuration cluster.

It provides access to the signal frequency, sampling frequency, number
of samples, noise amplitude, and noise threshold.

**Why Unbundle by Name?**

It allows individual cluster elements to be accessed using their names
rather than relying on their positions.

### Step 2: Generating the Original Sine Signal

A For Loop is used to generate the original sine wave.

The iteration terminal (`i`) represents the sample index.

The sine-wave equation is implemented using numeric functions, the Pi
constant, and the Sine function.

The output tunnel uses auto-indexing to collect the generated samples
into a 1D array.

**Why use a For Loop?**

A For Loop executes a fixed number of times. Since the number of samples
is known, it is suitable for generating the sine-wave samples.

**What is Auto-Indexing?**

Auto-indexing collects one output value from each loop iteration and
builds an array automatically.

### Step 3: Generating Random Noise

Another For Loop is used to generate random noise samples.

The following functions are used:

-   Random Number (0--1)
-   Multiply
-   Subtract
-   Multiply

The formula implemented is:

$$ v$$n$$=A(2r$$n$$-1) $$

The output tunnel uses auto-indexing to collect all the generated noise
samples into a 1D array.

Both For Loops use the same number of samples to ensure that the
original signal and noise arrays have matching lengths.

### Step 4: Adding Noise to the Original Signal

The Add function receives the original signal array and random noise
array.

It performs element-wise addition to generate the noisy signal array.

$$ x$$n$$=s$$n$$+v$$n$$ $$

Both arrays must have compatible dimensions and sizes.

### Step 5: Displaying Both Signals on One Waveform Graph

The Build Array function is used to combine the original signal array
and noisy signal array.

The **Concatenate Inputs** option is kept unchecked.

This produces a 2D array containing two rows:

-   Row 0: Original signal samples
-   Row 1: Noisy signal samples

The resulting 2D array is connected to a single Waveform Graph, allowing
both signals to be compared simultaneously.

**Why use a Waveform Graph instead of a Waveform Chart?**

A Waveform Graph displays a complete set of data at once, whereas a
Waveform Chart is generally used for continuously updating data.

Since both arrays are generated before plotting, a Waveform Graph is
suitable for this experiment.

### Step 6: Calculating Average Absolute Noise

The noise array is connected to the Absolute Value (`Abs`) function.

The output is then connected to the Mean function.

The Mean function calculates the average of all absolute noise samples.

The result is connected to the Average Noise Level indicator.

### Step 7: Implementing Noise Detection

The Mean function output is compared with the Noise Threshold using the
Greater? function.

The Boolean output is connected to the selector terminal of a Case
Structure.

Two cases are implemented:

-   TRUE case: Displays `NOISY`.
-   FALSE case: Displays `NORMAL`.

The Case Structure output is connected to the Noise Status indicator.

------------------------------------------------------------------------

## 6. Functions and Their Purpose

  LabVIEW Function           Purpose
  -------------------------- ------------------------------------------------------
  For Loop                   Generates a fixed number of samples
  Iteration Terminal (`i`)   Provides the current sample index
  Pi Constant                Provides the mathematical constant π
  Multiply                   Performs multiplication in signal equations
  Divide                     Divides the calculated phase by sampling frequency
  Sine                       Generates sine-wave samples
  Random Number (0--1)       Generates random values
  Subtract                   Shifts random values to positive and negative ranges
  Add                        Adds noise to the original signal
  Build Array                Combines two arrays into a 2D array
  Abs                        Converts negative noise values into positive values
  Mean                       Calculates average absolute noise
  Greater?                   Compares noise level with threshold
  Case Structure             Selects NORMAL or NOISY output
  Unbundle by Name           Extracts individual elements from a cluster

------------------------------------------------------------------------

## 7. Important Concepts and Questions Explored

### Q1. Why did we use a For Loop for sine-wave generation?

A For Loop executes a fixed number of iterations. Each iteration
generates one sine-wave sample using the current iteration index.

### Q2. What is the role of the iteration terminal (`i`)?

The iteration terminal provides the current loop index, starting from
zero.

It is used as the sample index in the sine-wave equation.

### Q3. Why is sampling frequency required?

Sampling frequency determines how frequently a continuous-time signal is
sampled.

It is also used to calculate the time interval between consecutive
samples.

$$ T_s=`\frac{1}{F_s}`{=tex} $$

### Q4. Why do we use random noise between -A and +A?

Noise can have positive or negative values. The transformation of a
random number from 0--1 to -1--+1 allows us to generate noise in both
directions.

### Q5. Why are two separate For Loops used?

One For Loop generates the original sine signal, while the other
generates random noise.

Both loops execute the same number of iterations and generate separate
arrays.

### Q6. Why do both arrays need the same number of samples?

Element-wise addition requires compatible array dimensions. Matching
sample counts ensure that every original signal sample has a
corresponding noise sample.

### Q7. Why use Build Array instead of connecting both arrays separately to the graph?

A single Waveform Graph can display multiple plots when provided with a
suitable multidimensional array.

Build Array combines the two 1D arrays into a 2D array.

### Q8. Why should Concatenate Inputs be unchecked?

When Concatenate Inputs is unchecked, Build Array combines the two 1D
arrays as separate rows in a 2D array.

If concatenation is enabled, the arrays are joined into one longer 1D
array instead.

### Q9. Why use Abs before Mean?

Without Abs, positive and negative noise samples can cancel each other
during averaging.

Applying Abs first allows us to calculate the average magnitude of the
noise.

### Q10. How did we arrive at the expected value of 0.25?

Our noise amplitude is 0.5, and the generated noise is uniformly
distributed between -0.5 and +0.5.

The expected average absolute value for this distribution is:

$$ E$$\|v\|$$=`\frac{A}{2}`{=tex} $$

Therefore:

$$ E$$\|v\|$$=`\frac{0.5}{2}`{=tex}=0.25 $$

The actual result can vary slightly because of randomness.

### Q11. Why do we use the Greater? function?

It compares the average absolute noise level with the specified
threshold and produces a Boolean output.

TRUE means the noise exceeds the threshold; FALSE means it does not.

### Q12. Why use a Case Structure?

A Case Structure executes different code depending on a selector value.

Here, it displays NOISY for TRUE and NORMAL for FALSE.

### Q13. What is the difference between a Waveform Graph and a Waveform Chart?

A Waveform Graph displays a complete set of data, whereas a Waveform
Chart updates as new data arrives.

The Waveform Graph is used here because the signal arrays are generated
before plotting.

### Q14. Why use clusters?

Clusters group related controls and indicators, even when they have
different data types.

They help keep the Front Panel organized and improve the structure of
the VI.

------------------------------------------------------------------------

## 8. Testing and Observations

The following values were used for testing:

  Parameter                Value
  -------------------- ---------
  Signal Frequency         10 Hz
  Sampling Frequency     1000 Hz
  Number of Samples         1000
  Noise Amplitude            0.5
  Noise Threshold            0.3

### Test Cases

    Noise Amplitude   Threshold Expected Status
  ----------------- ----------- ------------------
                0.5         0.3 NORMAL (usually)
                0.5         0.1 NOISY (usually)
                0.5         0.4 NORMAL (usually)
                0.8         0.3 NOISY (usually)

The expected average absolute noise level is approximately 0.25 for an
amplitude of 0.5 and approximately 0.4 for an amplitude of 0.8.

Actual measurements may differ slightly because of the random nature of
the generated noise.

### Observations

-   Increasing noise amplitude increases the variation in the noisy
    signal.
-   The original sine signal remains unchanged when noise amplitude is
    modified.
-   The average absolute noise level increases with noise amplitude.
-   Changing the threshold changes the detection decision.
-   Both original and noisy signals can be compared on the same Waveform
    Graph.

------------------------------------------------------------------------

## 9. Learning Outcomes

Through this experiment, the following concepts were understood and
implemented:

1.  Generation of sine-wave samples using mathematical equations.
2.  Understanding sampling frequency and sampling interval.
3.  Generation of random noise with adjustable amplitude.
4.  Addition of noise to an original signal.
5.  Understanding 1D and 2D arrays.
6.  Using For Loops and auto-indexing.
7.  Combining multiple signals using Build Array.
8.  Displaying multiple plots on a Waveform Graph.
9.  Calculating average absolute noise using Abs and Mean.
10. Implementing threshold-based detection using comparison functions
    and Case Structures.
11. Organizing inputs and outputs using clusters.

## 10. Limitations and Future Improvements

-   The current experiment uses simulated random noise rather than noise
    acquired from an actual sensor.
-   Average absolute noise provides a measure of noise magnitude but
    does not provide a complete analysis of signal quality.
-   The current detection method uses a fixed threshold.
-   The experiment can be extended by implementing RMS noise
    calculation, Signal-to-Noise Ratio (SNR), and digital filtering.
-   A low-pass filter can be introduced to study how unwanted
    high-frequency noise can be reduced.

## 11. Conclusion

The Noise Detector experiment was successfully implemented using
LabVIEW. A sine wave was generated, random noise was introduced, and
both signals were displayed together for comparison.

The average absolute noise level was calculated and compared with a
predefined threshold to identify whether the signal was NORMAL or NOISY.

This experiment provided practical experience with basic DSP operations,
array processing, statistical calculations, and decision-making
structures in LabVIEW.

------------------------------------------------------------------------

## 12. Screenshots

Add the following screenshots to the `Screenshots` folder in this
experiment's GitHub directory.

  Screenshot filename            Content
  ------------------------------ ----------------------------------------
  `01_Front_Panel.png`           Input cluster and output indicators
  `02_Sine_Generation.png`       Original sine-wave For Loop
  `03_Noise_Generation.png`      Random noise generation
  `04_Signal_Addition.png`       Add and Build Array implementation
  `05_Noise_Detection.png`       Abs, Mean, Greater? and Case Structure
  `06_Final_Block_Diagram.png`   Complete VI implementation

**Technical note:** Average absolute noise is a measure of noise
magnitude, not SNR. The simulated noise in this experiment is uniformly
distributed, not Gaussian noise.
