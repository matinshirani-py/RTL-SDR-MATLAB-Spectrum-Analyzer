# 📡 Real-Time SDR Spectrum Analyzer using MATLAB

A real-time RF spectrum analysis application developed in **MATLAB App Designer** using an **RTL-SDR receiver**.

The application acquires IQ samples from the SDR receiver and provides real-time visualization of the received RF spectrum through **FFT Spectrum, Power Spectral Density (PSD), and Waterfall** displays.

---

## 📌 Overview

Software-Defined Radio (SDR) provides access to raw IQ samples from radio-frequency signals, enabling software-based signal processing and analysis.

In this project, a graphical spectrum analyzer was developed using **MATLAB App Designer** to acquire and process RF signals from an RTL-SDR receiver in real time.

The application allows the user to:

* Acquire real-time IQ samples from an RTL-SDR receiver
* Configure the center frequency
* Adjust the receiver gain
* Start and stop signal acquisition
* Perform FFT-based spectrum analysis
* Calculate and display Power Spectral Density (PSD)
* Visualize the frequency spectrum over time using a Waterfall plot

The project was developed as an introduction to practical **Software-Defined Radio, RF signal acquisition, and digital signal processing**.

---

## ✨ Features

### 📊 Real-Time Spectrum

The application performs an FFT on the received IQ samples and displays the magnitude spectrum around the selected center frequency.

### 📈 Power Spectral Density

The Power Spectral Density is calculated using MATLAB's `periodogram` function and displayed in logarithmic scale.

### 🌊 Waterfall Visualization

A continuously updated waterfall plot provides a time-frequency representation of the received RF signal.

This allows changes in signal activity, stability, and frequency occupancy to be observed over time.

### 🎚️ Center Frequency Control

The user can dynamically specify the receiver's center frequency in MHz.

### 🔊 Gain Control

A slider is provided to control the RTL-SDR tuner gain.

### ⏯️ Acquisition Control

A Power Switch starts and stops the real-time acquisition and processing loop.

---

## 🛠️ Technologies

| Category             | Technology                  |
| -------------------- | --------------------------- |
| Programming Language | MATLAB                      |
| GUI Framework        | MATLAB App Designer         |
| SDR Hardware         | RTL-SDR                     |
| Signal Type          | Complex IQ Samples          |
| Signal Processing    | FFT, FFT Shift, Periodogram |
| Visualization        | Spectrum, PSD, Waterfall    |
| Sampling Rate        | 2.4 MS/s                    |

---

## 🧠 System Architecture

```text
             ┌─────────────────┐
             │     RTL-SDR     │
             │    Receiver     │
             └────────┬────────┘
                      │
                      │ IQ Samples
                      ▼
             ┌─────────────────┐
             │ MATLAB SDR      │
             │ RTL Receiver    │
             └────────┬────────┘
                      │
                      ▼
             ┌─────────────────┐
             │ Signal          │
             │ Processing      │
             └────────┬────────┘
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
       FFT /       Periodogram   FFT Frames
      Spectrum         │            │
          │            │            │
          ▼            ▼            ▼
      Spectrum        PSD       Waterfall
       Plot           Plot          Plot
```

---

## 🔬 Signal Processing Pipeline

The application follows the following processing chain:

```text
RF Signal
   ↓
RTL-SDR Receiver
   ↓
IQ Sample Acquisition
   ↓
FFT Processing
   ↓
┌──────────────┬──────────────┬──────────────┐
│              │              │
▼              ▼              ▼
Spectrum       PSD         Waterfall
```

### 1. IQ Data Acquisition

The RTL-SDR receiver is initialized using MATLAB's SDR receiver interface.

The current implementation uses:

```matlab
SampleRate = 2.4e6
SamplesPerFrame = 4096
```

The received samples are complex IQ data.

---

### 2. FFT Spectrum Analysis

The received signal is transformed from the time domain to the frequency domain using the Fast Fourier Transform:

```matlab
fftData = abs(fftshift(fft(data)));
```

`fftshift` is used to center the frequency spectrum around the selected center frequency.

The frequency axis is then calculated using the configured sample rate and center frequency.

---

### 3. Power Spectral Density

The PSD is estimated using MATLAB's `periodogram` function:

```matlab
[pxx,ff] = periodogram(data,[],N,sr,'centered');
```

The resulting power spectrum is displayed in logarithmic scale:

```matlab
10*log10(pxx)
```

This representation makes it easier to analyze signal power, noise, and interference across frequency.

---

### 4. Waterfall Display

For every received frame, the FFT result is appended to a history matrix:

```matlab
app.waterfallData = [app.waterfallData; fftData.'];
```

The application maintains a fixed history of recent FFT frames and displays them using `imagesc`.

This produces a time-frequency visualization of the received RF environment.

---

## 🖥️ Graphical User Interface

The application provides three main visualization areas:

### Spectrum

Displays the instantaneous frequency-domain magnitude of the received signal.

### PSD

Displays the estimated Power Spectral Density in dB.

### Waterfall

Displays the evolution of the spectrum over time.

The GUI also contains:

* Center Frequency input
* Gain slider
* Power switch

---

## ⚙️ Configuration

The default center frequency in the application is:

```text
100 MHz
```

The center frequency can be changed through the GUI.

The receiver uses a sampling rate of:

```text
2.4 MS/s
```

and processes:

```text
4096 samples/frame
```

---

## 🚀 Getting Started

### Requirements

* MATLAB
* MATLAB App Designer
* Communications Toolbox / SDR support required by the RTL-SDR interface
* RTL-SDR receiver
* Compatible RTL-SDR drivers

### Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/RTL-SDR-MATLAB-Spectrum-Analyzer.git
```

Open MATLAB and navigate to the project directory.

Open the App Designer project:

```text
project_ph1.mlapp
```

Connect the RTL-SDR receiver to the computer.

Run the application and select the desired center frequency and gain.

---

## 📡 Example

For example, if the center frequency is configured to:

```text
100 MHz
```

the application acquires RF samples around this frequency and displays:

* Frequency-domain spectrum
* Power Spectral Density
* Time-frequency waterfall

The center frequency can be changed directly from the GUI.

---

## 📚 Concepts Demonstrated

This project demonstrates practical implementation of:

* Software-Defined Radio (SDR)
* RF signal acquisition
* Complex IQ sampling
* Digital Signal Processing
* Fast Fourier Transform (FFT)
* Frequency-domain analysis
* Power Spectral Density estimation
* Periodogram
* Time-frequency analysis
* Real-time data visualization
* MATLAB App Designer
* RTL-SDR interfacing

---

## 🎯 Project Objectives

The main objectives of the project were:

1. Develop a graphical interface for an RTL-SDR receiver.
2. Acquire RF signals in real time.
3. Process IQ samples using digital signal-processing techniques.
4. Visualize the received spectrum using FFT.
5. Estimate and display Power Spectral Density.
6. Implement a real-time waterfall display.
7. Provide interactive control over center frequency and receiver gain.

---

## 🔮 Future Improvements

Possible future extensions include:

* Signal peak detection
* Automatic signal detection
* Frequency markers
* Adjustable FFT size
* Configurable sampling rate
* Noise-floor estimation
* Signal bandwidth estimation
* Recording IQ samples
* Signal classification
* Spectrogram customization
* Support for additional SDR hardware


