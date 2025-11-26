# DSB--SC-MODULATION-AND-DEMODULATION-USING-PYTHON

__AIM__:

To generate a Double Sideband Suppressed Carrier (DSB-SC) signal in Python (Google Colab), transmit it (optionally add noise), and recover the message using coherent (synchronous) demodulation with a low-pass filter. Observe time and frequency domain waveforms and measure demodulation performance

__APPARATUS REQUIRED__:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)

__Theory__:

DSB-SC signal: s(t) = m(t) · cos(2πf_c t)
Coherent demodulation: multiply received s(t) by a synchronized carrier cos(2πf_c t) then low-pass filter (LPF) to remove double-frequency components:

r(t) = s(t)·cos(2πf_c t) = m(t)·cos²(2πf_c t) = 0.5 m(t) + 0.5 m(t)·cos(4πf_c t)
LPF extracts 0.5·m(t) → scale by 2 to recover m(t).

__Procedure__:

1) Import libraries and set parameters
2) Define message and carrier signals
3) Generate DSB-SC signal (modulation)
4) View spectra (FFT) of message and DSB-SC
5) (Optional) Add noise
6) Coherent demodulation (multiply by synchronized carrier)
7) Low-pass filter to recover message


##program 

```c
import numpy as np
import matplotlib.pyplot as plt

# -----------------------
# Signal Parameters
# -----------------------
fs = 10000  # Sampling frequency
t = np.arange(0, 0.01, 1/fs)

Am = 1       # Amplitude of message signal
Ac = 5       # Amplitude of carrier signal
fm = 100     # Frequency of message signal
fc = 1000    # Frequency of carrier signal
mod_index = 0.7  # Modulation Index

# -----------------------
# Generate Signals
# -----------------------
message = Am * np.sin(2 * np.pi * fm * t)
carrier = Ac * np.sin(2 * np.pi * fc * t)

# -----------------------
# AM Modulation
# -----------------------
am_signal = Ac * (1 + mod_index * np.sin(2 * np.pi * fm * t)) * np.sin(2 * np.pi * fc * t)

# -----------------------
# Envelope Detection (Demodulation)
# -----------------------
# Rectifier (Full-wave absolute value)
rectified = np.abs(am_signal)

# Low-pass filter (moving average)
N = 50
demodulated = np.convolve(rectified, np.ones(N)/N, mode='same')

# -----------------------
# Plotting
# -----------------------
plt.figure(figsize=(12,10))

plt.subplot(4,1,1)
plt.plot(t, message)
plt.title("Message Signal (Modulating Signal)")
plt.xlabel("Time")
plt.ylabel("Amplitude")

plt.subplot(4,1,2)
plt.plot(t, carrier)
plt.title("Carrier Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")

plt.subplot(4,1,3)
plt.plot(t, am_signal, color='red')
plt.title("AM Modulated Signal")
plt.xlabel("Time")
plt.ylabel("Amplitude")

plt.subplot(4,1,4)
plt.plot(t, demodulated, color='green')
plt.title("Demodulated Signal (Recovered Message)")
plt.xlabel("Time")
plt.ylabel("Amplitude")

plt.tight_layout()
plt.show()
```
   __Tabulation__:
   
   ![IMG_20251126_120732](https://github.com/user-attachments/assets/af6f0f6b-1dc9-452f-bed3-5fab499ad877)


   __Output__:
   
   <img width="791" height="558" alt="image" src="https://github.com/user-attachments/assets/76444afd-f590-4167-851a-98f1953bca0d" />


   __Result__:
   
   Thus the  Double Sideband Suppressed Carrier (DSB-SC) signal was successfully implemented using python
