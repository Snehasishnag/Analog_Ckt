# 3-Band Active Audio Equalizer and Mixer Simulation

A complete circuit design and simulation of a multi-stage active audio crossover and equalizer, built and verified using LTspice. This project demonstrates analog signal routing, active RC filter design, and the use of voltage-controlled switches to simulate manual hardware interactions.

## 📌 Project Overview
The goal of this project was to design an analog audio mixer capable of splitting an input audio signal into three distinct frequency bands (Bass, Mid, Treble), applying independent and manually selectable gain to each band, and summing them back together without unwanted distortion.

## 🏗️ System Architecture

### 1. Preamplifier Stage
The raw input audio signal is fed into a Class-A common-emitter amplifier built around a standard NPN transistor. This provides an initial voltage boost to the signal before it enters the crossover network.
* **Base Gain:** ~4.7x (configured via a 4.7kΩ collector and 1kΩ emitter resistor).

### 2. Active Crossover Network (Frequency Splitting)
The pre-amplified signal is split into three parallel paths using passive RC filter networks. The cutoff frequencies ($f_c$) are determined by the standard filter equation $f_c = \frac{1}{2\pi RC}$.

* **Low Pass Filter (LPF) - Bass Frequencies:**
  * **Range:** 0 Hz to ~234 Hz
  * **Design:** 6.8kΩ resistor / 100nF capacitor network.
* **Band Pass Filter (BPF) - Midrange Frequencies:**
  * **Range:** ~284 Hz to ~3.3 kHz
  * **Design:** Cascaded HPF (3.9kΩ / 100nF) and LPF (4.7kΩ / 10nF) networks.
* **High Pass Filter (HPF) - Treble Frequencies:**
  * **Range:** ~4 kHz and above
  * **Design:** 3.9kΩ resistor / 10nF capacitor network.

### 3. Selectable Gain Control (Emitter Degeneration)
Each filtered band passes through its own dedicated NPN amplifier stage with a baseline voltage gain ($A_v \approx \frac{R_C}{R_E}$) of ~4.7x. 

To achieve dynamic control, a manual switching network is implemented using LTspice’s Voltage-Controlled Switch (`SW`) component.
* Toggling the control voltages (acting as physical hardware switches) swaps secondary emitter resistors into the circuit in parallel.
* This lowers the total emitter resistance, decreasing emitter degeneration and stepping the gain UP for that specific frequency band independently.

### 4. Summing Amplifier
The three processed signals (Low, Mid, High) are routed into an Operational Amplifier configured as an inverting summing amplifier. 
* **Design:** 10kΩ resistors are used across all inputs and the feedback loop.
* **Result:** Maintains a flat 1x gain, purely mixing the customized frequency bands back into a single, cohesive output waveform.

## 📊 Simulation & Verification
This design was verified using two primary SPICE simulations:

1. **Transient Analysis (`.tran 10m`)**
   * Observed real-time time-domain waveforms.
   * Verified that 1kHz sine wave inputs were successfully amplified, split, and summed without clipping over a 10-millisecond window.
2. **AC Analysis (`.ac dec 100 20Hz 20kHz`)**
   * Generated a Bode plot to measure the magnitude and phase response.
   * Confirmed the exact crossover frequencies of the LPF, BPF, and HPF networks across the human hearing spectrum.

## 🚀 How to Run the Simulation
1. Clone or download this repository.
2. Open the `.asc` schematic file in **LTspice**.
3. To view the time-domain waveforms, ensure the SPICE directive `.tran 10m` is active and click **Run**. Click the `V(in)` and `V(out)` nodes to compare the signals.
4. To view the frequency response, comment out the `.tran` directive, uncomment the `.ac` directive, and click **Run**.

---
*Designed by [Snehasish Nag] - 2k26*
