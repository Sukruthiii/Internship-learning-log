# Experiment 2 – Noise Detector

## Overview
This experiment demonstrates basic Digital Signal Processing (DSP) concepts using LabVIEW. A sine wave is generated, random noise is added, and the average absolute noise level is calculated to determine whether the signal exceeds a predefined noise threshold.

## Objectives
- Generate a sine wave using a For Loop.
- Generate random noise with adjustable amplitude.
- Combine the original signal and noise.
- Display original and noisy signals on the same Waveform Graph.
- Calculate average absolute noise.
- Implement threshold-based noise detection.

## Tools and Functions Used
- For Loop
- Sine Function
- Random Number (0–1)
- Multiply, Subtract and Add
- Build Array
- Absolute Value (Abs)
- Mean
- Greater? Comparison
- Case Structure
- Clusters
- Waveform Graph

## Mathematical Equations

**Original Signal:**
s[n] = sin(2πfn/Fs)

**Random Noise:**
v[n] = A(2r[n] - 1)

**Noisy Signal:**
x[n] = s[n] + v[n]

**Average Absolute Noise:**
N_avg = (1/N) Σ |v[n]|

## Noise Detection
The average absolute noise level is compared with a predefined threshold.

- If Average Noise > Threshold: NOISY
- Otherwise: NORMAL

## Expected Learning Outcomes
- Understanding sine-wave generation and random noise.
- Understanding array operations and auto-indexing.
- Understanding noise addition and basic noise analysis.
- Applying comparison functions and Case Structures.
- Using clusters to organize LabVIEW applications.

## Repository Contents
- Noise_Detector.vi – LabVIEW implementation.
- Screenshots – Front Panel and Block Diagram.
- Documentation– Experiment documentation.
