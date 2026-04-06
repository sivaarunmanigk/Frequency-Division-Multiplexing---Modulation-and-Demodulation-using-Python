# Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

__Aim__:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.


__Apparatus Required__:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)


__Theory__:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

__Procedure__:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband
PROGRAM:

    import numpy as np
    import matplotlib.pyplot as plt
    from scipy.signal import butter, filtfilt
    
    # Sampling
    fs = 50000
    t = np.arange(0, 0.01, 1/fs)
    
    # Message signals
    fm1 = 200
    fm2 = 400
    
    m1 = np.cos(2 * np.pi * fm1 * t)
    m2 = np.cos(2 * np.pi * fm2 * t)
    
    # Carrier frequencies (separated)
    fc1 = 3000
    fc2 = 7000
    
    c1 = np.cos(2 * np.pi * fc1 * t)
    c2 = np.cos(2 * np.pi * fc2 * t)
    
    # -------- MODULATION --------
    s1 = m1 * c1
    s2 = m2 * c2
    
    # FDM signal
    fdm = s1 + s2
    
    # -------- DEMODULATION --------
    def lowpass(signal, cutoff, fs):
        b, a = butter(5, cutoff/(0.5*fs), btype='low')
        return filtfilt(b, a, signal)
    
    # Recover m1
    demod1 = fdm * c1
    rec1 = lowpass(demod1, cutoff=500, fs=fs)
    
    # Recover m2
    demod2 = fdm * c2
    rec2 = lowpass(demod2, cutoff=500, fs=fs)
    
    # -------- PLOTTING --------
    plt.figure(figsize=(12, 10))
    
    plt.subplot(5,1,1)
    plt.plot(t, m1)
    plt.title("Message Signal 1")
    
    plt.subplot(5,1,2)
    plt.plot(t, m2)
    plt.title("Message Signal 2")
    
    plt.subplot(5,1,3)
    plt.plot(t, fdm)
    plt.title("FDM Signal")
    
    plt.subplot(5,1,4)
    plt.plot(t, rec1)
    plt.title("Recovered Signal 1")
    
    plt.subplot(5,1,5)
    plt.plot(t, rec2)
    plt.title("Recovered Signal 2")
    
    plt.tight_layout()
    plt.show()
__Output_:
<img width="940" height="557" alt="image" src="https://github.com/user-attachments/assets/c374c4a7-b8f0-4499-9028-bc5c9f84871a" />
<img width="940" height="557" alt="image" src="https://github.com/user-attachments/assets/71b1078a-11ad-446d-9ad0-224f71b994ae" />
<img width="940" height="557" alt="image" src="https://github.com/user-attachments/assets/f5b482a2-f347-40a5-866f-ff7bed83f6e1" />
TABULATION:
![20260406_182526](https://github.com/user-attachments/assets/669fbdce-6dbe-4d28-83a7-7a79725c8326)
![20260406_182531](https://github.com/user-attachments/assets/164c6b53-e50e-42da-a5e0-d7d557fdeaa0)

__Result__:
![20260406_182623](https://github.com/user-attachments/assets/ffa9374c-a534-436b-9d30-dbc97d0d287a)
