# Implementation of Filters and Analysis of Their Performance (LabVIEW)

A LabVIEW virtual instrument (VI) that generates a noisy multi-tone signal, filters it with **Lowpass, Highpass, Bandpass and Bandstop** IIR Butterworth filters, and compares the input and output in both the **time domain** and the **frequency domain**.

---

## Table of Contents

1. [Aim](#aim)
2. [Features](#features)
3. [How to Run](#how-to-run)
4. [Theory](#theory)
5. [Signal Generation](#1-signal-generation)
6. [Implementation](#2-implementation)
7. [Filter Operation and Analysis](#3-filter-operation-and-analysis)
8. [Test and Results](#4-test-and-results)
9. [Troubleshooting Notes](#5-troubleshooting-notes)
10. [Limitations and Future Work](#6-limitations-and-future-work)
11. [Conclusion](#7-conclusion)

---

## Aim

To generate a composite signal containing multiple frequency components and white noise, apply Lowpass, Highpass, Bandpass and Bandstop filters using LabVIEW, and analyse their performance in both the time domain and the frequency domain.

## Features

- Adjustable signal parameters (sampling frequency, duration, three tone frequencies, noise amplitude).
- Clean composite signal and noisy signal shown side by side.
- One enum control selects the filter type (Lowpass / Highpass / Bandpass / Bandstop).
- Cutoff frequencies are set from front-panel controls.
- Time-domain graphs for the clean, noisy and filtered signals.
- Frequency-domain graphs (FFT magnitude) for the noisy and filtered signals.



## How to Run

1. Install LabVIEW with the **Signal Processing** (Express) toolset available.
2. Open `SELF EXERCISE 1.lvproj` and then open `Filters.vi`.
3. On the front panel, set the signal parameters (defaults are in the table below).
4. Choose a **Filter Type** in the enum and set the cutoff frequencies.
5. Click **Run** (the arrow) and compare the time-domain and frequency-domain graphs.
6. Use a **linear** Y-axis on the graphs. A log axis hides the negative half of a waveform.

---

## Theory

| Concept | Meaning |
|---|---|
| Sampling frequency (Fs) | Number of samples per second. Must be more than twice the highest frequency in the signal. |
| Nyquist frequency | Fs / 2 = 500 Hz here. Anything above this would alias. |
| Frequency resolution | Fs / N = 1000 / 2000 = 0.5 Hz per FFT bin. |
| Butterworth filter | Has a maximally flat passband. Roll-off gets steeper as the order increases. |
| Cutoff frequency (fc) | Frequency where the gain falls to about 0.707 (-3 dB) of the passband gain. |

For an order-*n* Butterworth filter, the gain magnitude is:

**Lowpass**

$$|H(f)| = \frac{1}{\sqrt{1 + \left(\frac{f}{f_c}\right)^{2n}}}$$

**Highpass**

$$|H(f)| = \frac{1}{\sqrt{1 + \left(\frac{f_c}{f}\right)^{2n}}}$$

### Expected gain at each tone (order n = 4)

| Filter | fc | Gain at 10 Hz | Gain at 50 Hz | Gain at 100 Hz |
|---|---|---|---|---|
| Lowpass | 20 Hz | about 0.998 | about 0.026 | about 0.002 |
| Highpass | 30 Hz | about 0.012 | about 0.992 | about 1.000 |

These are theoretical values from the formulas above. Measured values will differ slightly because of the Hanning window, the start-up transient and the noise.

---

## 1. Signal Generation

A composite signal of three sine waves with different frequencies and amplitudes was generated. White noise was added to obtain the original noisy signal.

### Signal Parameters

| Parameter | Value |
|---|---:|
| Sampling Frequency (Fs) | 1000 Hz |
| Signal Duration (T) | 2 seconds |
| Frequency 1 (f1) | 10 Hz |
| Amplitude 1 | 1 |
| Frequency 2 (f2) | 50 Hz |
| Amplitude 2 | 0.7 |
| Frequency 3 (f3) | 100 Hz |
| Amplitude 3 | 0.5 |
| Noise Amplitude | 0.5 |

### Number of Samples

$$N = F_s \times T = 1000 \times 2 = 2000$$

### Composite Signal Equation

$$x(t) = \sin(2\pi \cdot 10\,t) + 0.7\sin(2\pi \cdot 50\,t) + 0.5\sin(2\pi \cdot 100\,t)$$

The clean signal therefore has a peak value of up to 2.2.

### Noise Generation

White noise was generated with the **Random Number (0-1)** function and scaled to roughly -0.5 to +0.5:

$$n[i] = (2\,r[i] - 1) \times A_{noise}$$

where $r[i]$ is the random number and $A_{noise}$ is the noise amplitude (0.5).

The noisy signal is:

$$x_{noisy}(t) = x(t) + n(t)$$

The noise is uniformly distributed. A new noise sequence is generated on every run, so the noisy signal differs slightly each time.

### Sample Time Calculation

$$t = \frac{i}{F_s}$$

where $i$ is the loop iteration index.

The arrays were converted into waveform data with **Build Waveform**:

- Initial time (t0) = 0
- Time interval (dt) = 1/Fs = 0.001 s

---

## 2. Implementation

### A. Front Panel

**1. Signal Parameters**

- Sampling Frequency
- Signal Duration
- Frequency 1, Frequency 2, Frequency 3
- Noise Amplitude

**2. Filter Parameters**

A *Filter Type* enum with four options: Lowpass, Highpass, Bandpass, Bandstop.

A *Filter Parameters* cluster provides:

- Cutoff Frequency
- Lower Cutoff Frequency
- Upper Cutoff Frequency

**3. Time-Domain Analysis**

- Clean Composite Signal
- Original Noisy Signal
- Filtered Signal

**4. Frequency-Domain Analysis**

- Original Noisy Signal - Frequency Spectrum
- Filtered Signal - Frequency Spectrum


### B. Block Diagram

**1. Signal generation (For Loop)**

A For Loop generates N = 2000 samples. Inside the loop:

1. The sample time is calculated from the iteration terminal and the sampling frequency.
2. Three sine waves (10, 50 and 100 Hz) are generated.
3. Their amplitudes are scaled to 1, 0.7 and 0.5.
4. The sine waves are added to give the clean composite signal.
5. Random noise is generated and scaled.
6. The noise is added to the composite signal to give the noisy signal.

Auto-indexing output tunnels collect the samples into arrays.

**2. Waveform construction**

Both arrays go through **Build Waveform** with dt = 1/Fs and are wired to their time-domain graphs.

**3. Spectrum of the original signal**

The **Spectral Measurements** Express VI analyses the noisy waveform.

| Setting | Configuration |
|---|---|
| Measurement | Magnitude (Peak) |
| Result | Linear |
| Window | Hanning |
| Averaging | Disabled |

The FFT magnitude output is wired to the *Original Noisy Signal - Frequency Spectrum* graph.

**4. Filter selection (Case Structure)**

The *Filter Type* enum is wired to the selector of a **Case Structure**. The noisy waveform is the input to every case, and each case contains a **Filter Express VI**.

| Filter Type | Cutoff Settings | Filter Design |
|---|---|---|
| Lowpass | 20 Hz | IIR Butterworth, Order 4 |
| Highpass | 30 Hz | IIR Butterworth, Order 4 |
| Bandpass | 40-60 Hz | IIR Butterworth, Order 4 |
| Bandstop | 40-60 Hz | IIR Butterworth, Order 4 |

The cutoff inputs are exposed on the block diagram and wired to the *Filter Parameters* cluster. The output of each case goes to one common output tunnel, so the same *Filtered Signal* graph shows whichever filter is selected.

**5. Spectrum of the filtered signal**

The common filtered waveform goes to a second Spectral Measurements Express VI with the same settings (Magnitude Peak, Linear, Hanning, no averaging), and its FFT magnitude is shown on the *Filtered Signal - Frequency Spectrum* graph.


---

## 3. Filter Operation and Analysis

### A. Lowpass Filter (fc = 20 Hz)

Passes frequencies below the cutoff and attenuates those above it.

| Frequency Component | Expected Result |
|---|---|
| 10 Hz | Passed |
| 50 Hz | Attenuated |
| 100 Hz | Attenuated |
| Noise | Mostly removed (the noise above 20 Hz) |

The time-domain output should be a clean 10 Hz sine of about ±1 amplitude, with 20 cycles in 2 s.

### B. Highpass Filter (fc = 30 Hz)

Passes frequencies above the cutoff and attenuates those below it.

Observed behaviour:

- The 10 Hz component was significantly reduced.
- The 50 Hz and 100 Hz components were retained.
- Higher-frequency noise components were also retained.

The frequency spectrum confirmed that the 10 Hz component was removed significantly.

### C. Bandpass Filter (40-60 Hz)

Passes frequencies inside the band and attenuates those outside it.

| Frequency Component | Expected Result |
|---|---|
| 10 Hz | Attenuated |
| 50 Hz | Passed |
| 100 Hz | Attenuated |

The spectrum should show a prominent 50 Hz peak, with the 10 Hz and 100 Hz peaks greatly reduced.

### D. Bandstop Filter (40-60 Hz)

Attenuates frequencies inside the band and passes those outside it.

| Frequency Component | Expected Result |
|---|---|
| 10 Hz | Passed |
| 50 Hz | Attenuated |
| 100 Hz | Passed |

The spectrum should show the 50 Hz peak greatly reduced while the 10 Hz and 100 Hz peaks remain.

### Summary comparison

| Filter | Keeps | Removes | Typical use |
|---|---|---|---|
| Lowpass | Low frequencies | High-frequency noise | Smoothing, noise removal |
| Highpass | High frequencies | Slow drift, DC offset | Removing baseline wander |
| Bandpass | One band | Everything outside the band | Isolating one tone |
| Bandstop | Everything outside the band | One band | Removing an interfering tone (for example mains hum) |

---

## 4. Test and Results

The VI was tested by selecting each filter type with the enum and observing the filtered waveform and frequency spectrum.

| Test | Filter Selection | Expected Observation | Status |
|---|---|---|---|
| 1 | Lowpass - 20 Hz | 10 Hz retained; 50 Hz and 100 Hz attenuated | Verified in the time domain (clean 10 Hz sine of about ±1) |
| 2 | Highpass - 30 Hz | 10 Hz attenuated; 50 Hz and 100 Hz retained | Verified in the spectrum |
| 3 | Bandpass - 40-60 Hz | 50 Hz retained; 10 Hz and 100 Hz attenuated | To be recorded |
| 4 | Bandstop - 40-60 Hz | 50 Hz attenuated; 10 Hz and 100 Hz retained | To be recorded |

> The results for tests 3 and 4 are the expected behaviour. Record the observations here after running them, and add screenshots to `images/`.



### How to check a result

- **Time domain:** count the cycles. Frequency = cycles ÷ time (20 cycles in 2 s = 10 Hz). Zoom to 0-0.5 s and measure the peak-to-peak period with cursors.
- **Frequency domain:** the spectrum shows a peak at each tone present. Compare the peak heights before and after filtering (expected near 1, 0.7 and 0.5 for the passed tones).

---

## 5. Troubleshooting Notes

Problems met while building this VI, and their fixes:

| Symptom | Cause | Fix |
|---|---|---|
| Filtered output was tiny (around 1E-8) or a slow curve | The filter's effective cutoff was far below the intended value, from a wrong terminal connection | Used the Filter Express VI, which takes the sampling rate from the waveform's dt |
| Filtered graph changed from run to run | `init/cont` set to True on the Butterworth Filter VI, so state carried over from the previous run | Leave it False for a one-shot VI, or use the Express VI |
| Negative half of the waveform was missing | Graph Y-axis was on a log scale | Set Y Scale → Mapping → Linear |
| Waveform looked like a solid band | 2 s of 1000 Hz data plotted at once | Zoom the X axis (for example 0-0.5 s) |

---

## 6. Limitations and Future Work

- Bandpass and bandstop results are still to be recorded and checked against the spectrum.
- Filter order and cutoffs are fixed; they could be made front-panel controls so their effect can be explored.
- Add a quantitative measure of performance, such as SNR before and after filtering, or the RMS error between the filtered and clean signals.
- Compare IIR Butterworth with other designs (Chebyshev, Elliptic) and with FIR filters.
- Show the phase response and group delay to quantify the delay introduced by the filters.
- Display all four filters at once instead of one at a time.
- Add a Gaussian noise option and a noise-level sweep.

---

## 7. Conclusion

A multi-frequency noisy signal generator was implemented in LabVIEW, and four digital filters were applied to it using Filter Express VIs. The Lowpass, Highpass, Bandpass and Bandstop filters are selected with an enum and a Case Structure, so one VI covers all four.

The performance of the filters was analysed using time-domain waveforms and frequency-domain FFT magnitude spectra. The time-domain graphs show how the waveform changes after filtering, and the spectra show which frequency components are passed or attenuated. The experiment demonstrates how different filters selectively pass or attenuate frequency components in a noisy signal.

---

