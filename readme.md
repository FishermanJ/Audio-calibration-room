# Audio EQ Single-Measurement Calibration

A standalone notebook for room acoustic measurement and automatic parametric EQ correction using a single log-sweep measurement.

## Requirements

```bash
pip install numpy scipy matplotlib sounddevice soundfile
```

---

## Pipeline Overview

```
Log Sweep  →  Play & Record  →  Deconvolution  →  Frequency Response
     →  Target Curve  →  Auto-EQ Filters  →  Verification  →  Final Comparison
```

---

## Step-by-Step

### 1. Log Sweep Generator

Generates a sine sweep from 20 Hz to 20 kHz. The sweep and its inverse filter are used to extract the room impulse response via deconvolution.

![Log Sweep](readme_imgs/plot_05_1.png)

---

### 2. Play & Record

Plays the sweep through the speakers and records the microphone response. A simulated mode is available (adds room reflections + noise) if no audio hardware is connected.

![Play & Record](readme_imgs/plot_07_1.png)

---

### 3. Deconvolution → Impulse Response → Frequency Response

Cross-correlates the recording with the inverse sweep filter to isolate the room impulse response, then converts to frequency domain via FFT.

![Impulse Response & Frequency Response](readme_imgs/plot_09_1.png)

---

### 4. Target Curve & Deviation

Computes the deviation between the measured frequency response and a smooth target curve (boosted bass shelf below 200 Hz, flat mid, gentle HF roll-off above 10 kHz).

![Target Curve & Deviation](readme_imgs/plot_11_0.png)

---

### 5. Auto-EQ: Measured vs Corrected (Predicted)

Fits up to 10 parametric peaking filters to the deviation curve. Shows the measured response, the predicted post-EQ response, and the flat target.

![Measured vs Corrected](readme_imgs/plot_15_0.png)

---

### 6. Final Comparison: Before / Predicted / Actual After EQ

Applies the computed EQ filters to a verification sweep recording and overlays all three curves: raw measurement, predicted correction, and actual corrected measurement.

![Before / Predicted / After EQ](readme_imgs/plot_19_1.png)

---

### 7. Music Playback: Room Coloring Snapshots

Plays a real music file through the speakers, records it with the microphone at configurable intervals, and compares the original spectrum against the captured spectrum. Reveals how the room and speaker system colors the sound.

![Room Coloring Snapshots](readme_imgs/plot_23_1.png)

---

## Configuration

All key parameters are in the **Imports & Configuration** cell:

| Parameter | Default | Description |
|-----------|---------|-------------|
| `SAMPLE_RATE` | 44100 | Audio sample rate (Hz) |
| `SWEEP_DURATION` | 5.0 s | Length of the log sweep |
| `F_START` / `F_END` | 20 / 20000 Hz | Sweep frequency range |
| `N_FILTERS` | 10 | Max parametric EQ bands |
| `SIMULATE` | `True` | Use simulated room instead of real mic |

Set `SIMULATE = False` and plug in a measurement microphone to run a real room calibration.

---

## Output

- Parametric EQ filter list (center frequency, gain dB, Q) — ready to paste into any DAW or EQ plugin
- Before/after frequency response plots
- Room coloring spectrum comparison plots
