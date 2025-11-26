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
from scipy.signal import butter, lfilter

# Parameters (same as your code)
fs = 10000 # Sampling frequency in Hz
t = np.arange(0, 1, 1/fs)  # Time array for 1 second
fm = 100  # Message frequency in Hz
fc = 1000   # Carrier frequency in Hz
Am = 11.1   # Amplitude of message
Ac = 22.2   # Amplitude of carrier

# Message and carrier signals
message = Am * np.cos(2 * np.pi * fm * t)
carrier = Ac * np.cos(2 * np.pi * fc * t)

# DSB-SC modulation
dsb_sc = message * carrier

# Demodulation: multiply again by carrier (coherent detection)
demodulated_signal = dsb_sc * carrier

# Low-pass filter design to extract message from demodulated signal
def butter_lowpass(cutoff, fs, order=5):
    nyq = 0.5 * fs
    normal_cutoff = cutoff / nyq
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return b, a

def lowpass_filter(data, cutoff, fs, order=5):
    b, a = butter_lowpass(cutoff, fs, order=order)
    y = lfilter(b, a, data)
    return y

cutoff_freq = 1500  # cutoff frequency for low pass filter (higher than message frequency)
rec_message = lowpass_filter(demodulated_signal, cutoff_freq, fs)

# Plotting
plt.figure(figsize=(12, 9))

plt.subplot(4, 1, 1)
plt.plot(t, message)
plt.title("Original Message Signal")
plt.xlabel("Time (seconds)")

plt.subplot(4, 1, 2)
plt.plot(t, dsb_sc)
plt.title("DSB-SC Modulated Signal")
plt.xlabel("Time (seconds)")

plt.subplot(4, 1, 3)
plt.plot(t, demodulated_signal)
plt.title("Signal After Coherent Demodulation (before LPF)")
plt.xlabel("Time (seconds)")

plt.subplot(4, 1, 4)
plt.plot(t, rec_message)
plt.title("Recovered Message Signal (after Low-Pass Filter)")
plt.xlabel("Time (seconds)")

plt.tight_layout()
plt.show()
```
   __Tabulation__:
   
   ![IMG_20251126_120732](https://github.com/user-attachments/assets/af6f0f6b-1dc9-452f-bed3-5fab499ad877)


   __Output__:
   
   <img width="791" height="558" alt="image" src="https://github.com/user-attachments/assets/76444afd-f590-4167-851a-98f1953bca0d" />


   __Result__:
   
   Thus the  Double Sideband Suppressed Carrier (DSB-SC) signal was successfully implemented using python
