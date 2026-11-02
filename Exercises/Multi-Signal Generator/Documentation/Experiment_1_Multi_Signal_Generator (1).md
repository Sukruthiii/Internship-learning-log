# Experiment 1 – Multi-Signal Generator Using LabVIEW

## 1. Aim

To design a LabVIEW VI that generates three types of periodic signals—sine, square, and triangle waves—and displays the selected waveform with its timing information.

## 2. Project structure

Keep the experiment organized in these folders:

```text
Experiment 1 - Multi-Signal Generator/
├── Main VI/
│   └── Multi-Signal Generator.vi
├── Controls/
│   ├── Signal Type.ctl
│   └── Signal Settings.ctl
├── SubVIs/
└── Documentation/
    └── Experiment 1 - Multi-Signal Generator.md
```

## 3. Front Panel design

The Front Panel contains a `Signal Settings` cluster and two output indicators.

### 3.1 Signal Type Enum

An Enum named `Signal Type` provides three choices:

- Sine Wave
- Square Wave
- Triangle Wave

An Enum is useful when the user must select one item from a fixed list. It avoids manually entering numeric codes or using multiple Boolean controls.

The Enum was saved as the Type Definition `Signal Type.ctl`.

### 3.2 Signal Settings cluster

The `Signal Settings` cluster groups the following parameters:

| Element | Data type | Purpose |
|---|---|---|
| Signal Type | Enum | Selects the waveform |
| Amplitude | DBL | Sets the peak amplitude |
| Frequency (Hz) | DBL | Sets the signal frequency |
| Sampling Frequency (Hz) | DBL | Sets samples per second |
| Number of Samples | I32 | Sets the number of samples to generate |

The cluster was saved as the Type Definition `Signal Settings.ctl`.

A cluster groups different data types into one logical unit. A Type Definition makes a reusable control available to other VIs and helps keep its instances consistent.

### 3.3 Output indicators

- `Generated Signal`: a Waveform Graph that displays the generated waveform.
- `Signal Status`: a String Indicator that displays a message such as `Sine Wave Generated`.

The Front Panel is arranged with controls on the left and indicators on the right.

## 4. Block Diagram implementation

### Step 1 – Extract the cluster elements

The Signal Settings cluster is wired to `Unbundle By Name`.

This function extracts the five named elements: Signal Type, Amplitude, Frequency, Sampling Frequency, and Number of Samples.

Each value can then be wired separately to the part of the diagram that needs it.

### Step 2 – Set up the For Loop

A For Loop is used to calculate one sample per iteration.

The `Number of Samples` output is wired to the loop's `N` terminal. For example, if N is 100, the loop executes 100 times.

The loop iteration terminal `i` starts at zero and increments by one. It provides the sample index $n$.

### Step 3 – Calculate phase

The phase for each sample is calculated using:

$$

\theta[n] = \frac{2\pi f n}{F_s}

$$

where:

- $\theta[n]$ is the phase in radians.
- $f$ is the signal frequency in hertz.
- $n$ is the current sample index from the For Loop.
- $F_s$ is the sampling frequency in samples per second.

Multiply and Divide functions, together with the Pi constant, implement this calculation.

The phase is shared by the waveform-generation cases.

### Step 4 – Select the waveform

A Case Structure is placed inside the For Loop. The `Signal Type` Enum output is wired to its selector terminal (`?`).

The cases are:

- Sine Wave
- Square Wave
- Triangle Wave

Only the selected case executes on each loop iteration.

## 5. Waveform generation

### 5.1 Sine wave

The equation is:

$$

x[n] = A\sin\left(\frac{2\pi f n}{F_s}\right)

$$

Connections:

1. Wire the calculated phase to the Sine function.
2. Wire the Sine output and Amplitude to a Multiply function.
3. Wire the result to the numeric output tunnel of the Case Structure.

The Sine function returns values between -1 and +1. Multiplying by amplitude A scales the peak values to +A and -A.

### 5.2 Square wave

The square wave switches between +A and -A:

$$

x[n] =
\begin{cases}
+A, & \sin(\theta[n]) \geq 0\\
-A, & \sin(\theta[n]) < 0
\end{cases}

$$

Connections:

1. Wire the phase to a Sine function.
2. Compare the Sine output with zero using `Greater Or Equal`.
3. Multiply Amplitude by -1 to obtain -A.
4. Wire the comparison's Boolean output to the selector input of `Select`.
5. Wire +A to the T input and -A to the F input.
6. Wire the Select output to the same numeric Case Structure output tunnel.

`Greater Or Equal` produces TRUE when the sine value is at least zero and FALSE otherwise. `Select` chooses T when its selector is TRUE and F when it is FALSE.

### 5.3 Triangle wave

The triangle wave is generated using:

$$

x[n] = \frac{2A}{\pi}\sin^{-1}(\sin(\theta[n]))

$$

Connections:

1. Wire the phase to a Sine function.
2. Wire the Sine output to an Inverse Sine function.
3. Multiply the Inverse Sine output by twice the amplitude.
4. Divide the result by Pi.
5. Wire the result to the same numeric output tunnel.

The Inverse Sine function returns the principal inverse-sine value in radians. This expression produces a repeating triangular waveform with peak values +A and -A.

### Step 5 – Collect the samples

The numeric output of the Case Structure is wired to an output tunnel of the For Loop.

Auto-indexing is enabled at the loop output. It collects one numeric value from each iteration and builds a one-dimensional array of samples.

For 100 iterations, the array contains 100 samples.

## 6. Build Waveform function

The sample array is wired to the `Y` input of `Build Waveform`.

Build Waveform packages the sample values together with timing information so that the Waveform Graph can use a meaningful time axis.

Its main inputs are:

- **Y:** the array of sample values.
- **dt:** the time interval between consecutive samples.
- **t0:** the starting time of the waveform.

The sampling interval is:

$$

dt = \frac{1}{F_s}

$$

For $F_s = 100$ Hz:

$$

dt = \frac{1}{100} = 0.01\text{ s}

$$

A constant of zero is wired to `t0`, meaning the first sample occurs at time zero.

The Build Waveform output is wired to the `Generated Signal` Waveform Graph.

An ordinary array contains sample values but does not itself specify the sample interval or starting time. Build Waveform packages these values with that timing information.

## 7. Signal Status indicator

A second Case Structure is placed **outside the For Loop**. The Signal Type Enum output is wired to its selector terminal.

Each case contains a String Constant:

| Case | String Constant |
|---|---|
| Sine Wave | `Sine Wave Generated` |
| Square Wave | `Square Wave Generated` |
| Triangle Wave | `Triangle Wave Generated` |

Each String Constant is wired to the same output tunnel. The output tunnel is wired to the `Signal Status` String Indicator.

This Case Structure is separate from the numeric waveform-generation Case Structure. The first produces numeric samples; the second produces a text message. Keeping them separate avoids mixing incompatible data types.

## 8. Sampling concepts

### Sampling frequency

Sampling frequency $F_s$ is the number of samples generated or acquired per second. For example, 100 Hz means 100 samples per second.

### Sampling interval

The time between samples is:

$$

dt = \frac{1}{F_s}

$$

At 100 Hz, the interval is 0.01 seconds.

### Sample time

The time associated with sample index $n$, when the first sample starts at zero, is:

$$

t[n] = \frac{n}{F_s}

$$

For 100 Hz, the first few sample times are 0, 0.01, 0.02, 0.03 seconds, and so on.

### Signal frequency and time period

Signal frequency $f$ is the number of cycles per second. The period of one cycle is:

$$

T = \frac{1}{f}

$$

For a 5 Hz signal, one cycle takes 0.2 seconds.

### Samples per cycle

$$

\text{Samples per cycle} = \frac{F_s}{f}

$$

For a 5 Hz signal sampled at 100 Hz:

$$

\frac{100}{5} = 20

$$

There are 20 samples per cycle.

### Duration and last sample time

The nominal record duration is:

$$

\text{Duration} = \frac{N}{F_s}

$$

For 100 samples at 100 Hz, this is 1 second.

However, if the first sample is at time zero, the last sample occurs at:

$$

t_{\text{last}} = \frac{N-1}{F_s}

$$

For 100 samples at 100 Hz, the last sample is at 0.99 seconds.

### Number of cycles

$$

\text{Cycles} = \frac{Nf}{F_s}

$$

For 100 samples, a 5 Hz signal, and a 100 Hz sampling frequency:

$$

\frac{100 \times 5}{100} = 5\text{ cycles}

$$

## 9. Nyquist sampling theorem

For a band-limited signal, the sampling frequency must be greater than twice the highest frequency component to allow ideal reconstruction:

$$

F_s > 2f_{\max}

$$

The theoretical boundary is called the Nyquist rate. In practical systems, sampling exactly at the boundary is not a good design choice. A higher sampling frequency is normally selected, and an anti-aliasing filter is used before sampling to limit frequencies above the intended measurement bandwidth.

For a 5 Hz sine wave, the theoretical Nyquist rate is 10 samples per second. A higher rate such as 50 Hz or 100 Hz gives more samples per cycle and a more detailed display.

A useful starting point for visually inspecting a sine wave is around 10–20 or more samples per cycle. This is a practical guideline, not a universal requirement for every measurement.

| Sampling frequency | Samples per cycle for a 5 Hz signal | General appearance |
|---:|---:|---|
| 10 Hz | 2 | Nyquist boundary; poor visual detail |
| 20 Hz | 4 | Coarse |
| 50 Hz | 10 | More detail |
| 100 Hz | 20 | Better visual detail |
| 200 Hz | 40 | More detailed |

A graph that joins sample points with lines can look continuous. The underlying data is still discrete. Showing point markers on the graph helps inspect the individual samples.

## 10. Sampling-frequency experiment

Use these settings as a starting point:

- Amplitude: 2
- Signal frequency: 5 Hz
- Number of samples: 100

Then change only the sampling frequency:

| Sampling frequency | Number of samples | Nominal duration N/Fs |
|---:|---:|---:|
| 20 Hz | 100 | 5 s |
| 50 Hz | 100 | 2 s |
| 100 Hz | 100 | 1 s |
| 200 Hz | 100 | 0.5 s |

Because N is fixed, increasing the sampling frequency shortens the time record. To compare the same one-second duration at each rate, change N as well:

- 50 Hz: 50 samples
- 100 Hz: 100 samples
- 200 Hz: 200 samples

This makes the comparison fairer because each graph represents the same duration.

## 11. Functions used and their purposes

| Function or structure | Purpose |
|---|---|
| Unbundle By Name | Extracts named values from the Signal Settings cluster |
| For Loop | Repeats the sample calculation |
| Case Structure | Executes the selected waveform-generation case |
| Sine | Calculates the sine of a phase angle |
| Inverse Sine | Calculates the principal inverse sine |
| Greater Or Equal | Compares a value with zero and returns a Boolean |
| Select | Chooses between positive and negative amplitude |
| Multiply | Performs amplitude scaling and other multiplication |
| Divide | Calculates phase and sample interval |
| Build Waveform | Packages samples with timing information |
| String Constant | Supplies a text status message |
| Waveform Graph | Displays the waveform |

## 12. Coding practices followed

- Group related inputs in a Cluster Type Definition.
- Use an Enum for a fixed list of waveform choices.
- Arrange the Front Panel with controls on the left and indicators on the right.
- Arrange Block Diagram dataflow from left to right.
- Use meaningful names for controls, indicators, and files.
- Keep numeric waveform generation separate from text status generation.
- Use auto-indexing to collect samples from the For Loop.
- Place Build Waveform outside the loop because the complete sample array is assembled after the loop finishes.
- Keep the project organized into Main VI, Controls, SubVIs, and Documentation folders.
- Avoid unnecessary wire crossings and overlapping objects.

## 13. Expected results

The VI is designed to generate the waveform selected by the user.

- Sine Wave: a periodic smooth waveform.
- Square Wave: a waveform switching between positive and negative amplitude.
- Triangle Wave: a periodic waveform with rising and falling linear sections.

For amplitude 2, the expected peak values are approximately +2 and -2.

For a 5 Hz signal sampled at 100 Hz with 100 samples, the record contains five cycles. Its nominal duration is one second, and its last sample is at 0.99 seconds.

The Signal Status indicator displays the corresponding message for the selected waveform.

## 14. Conclusion

The Multi-Signal Generator brings together LabVIEW programming concepts including Type Definitions, Enums, Clusters, Unbundle By Name, For Loops, Case Structures, mathematical functions, auto-indexing, and Build Waveform.

It also provides practical experience with DSP fundamentals: signal frequency, sampling frequency, sample interval, sample index, samples per cycle, record duration, and the Nyquist sampling theorem.

The experiment provides a foundation for further DSP work such as noise detection, signal filtering, signal amplification, sampling and aliasing simulation, and signal monitoring.

## 15. Screenshots to add

Add screenshots of the following to the `Documentation/Screenshots` folder:

1. Front Panel with Signal Settings and output indicators.
2. Signal Type Enum Type Definition.
3. Signal Settings Cluster Type Definition.
4. Main Block Diagram showing Unbundle By Name and the For Loop.
5. Sine Wave case.
6. Square Wave case.
7. Triangle Wave case.
8. Build Waveform and its connections to the Waveform Graph.
9. Signal Status Case Structure.
10. Final Front Panel showing each waveform after a successful run.

**Report note:** Before submitting, run the VI with all three signal selections and confirm that the graph and status message update correctly. Add actual screenshots and observations from your run rather than treating expected results as verified results.
