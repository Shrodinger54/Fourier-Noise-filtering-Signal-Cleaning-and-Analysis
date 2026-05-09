## **Summary**

This report outlines a numerical verification system design to isolate weak sinusoidal signals from high-variance Gaussian noise. Using **Fast Fourier Transform (FFT)**, the system successfully identified peak frequencies in corrupted datasets, and improved our SNR.

* **Signal Characterization and Synthesis**
  I begin by synthesizing a multicomponent signal representing a physical measurement.
* **Stochastic Noise Injection**
  To simulate real world sensor data, the clean signal is corrupted with a white Gaussian noise using (nump.random.randn).
<img width="632" height="388" alt="image" src="https://github.com/user-attachments/assets/e278180b-5142-4bc7-bc0e-09bcd7de370a" />

 **Impact** the noise obscures the underlying sinusoids in the time domain, making manual frequency estimation impossible.
 
**Spectral Analysis via FFT** the FFT is employed to transition from the time domain x(t) to the frequency domain X(f).

* **Digital Filtering and Reconstruction** to recover the original signal, a **frequency domain band-pass filter** is implemented
1. Masking: a logical mask is created to identify indices within the desired passband (e.g. 40 Hz to 60 Hz for a motor vibration)
2. frequencies outside this are zeroed out.
3. Inverse Transform: (np.fft.ifft) converts the cleaned spectrum back to the time domain, "scrubbing" the noise and isolating the target oscillation.


   **Cleaned Signal Below**

   
   <img width="640" height="401" alt="image" src="https://github.com/user-attachments/assets/0f486667-2585-4361-9a4d-2e602fa5debd" />



**Physical System and FFT Application
System Description:**
In this case, we model the variation in heart rate over time, which is often measured in ECG (Electrocardiogram) signals or through devices that track the heartbeats of a subject. The signal we use will have noise (e.g., due to measurement inaccuracies or external interference), and our goal is to clean the signal using FFT and analyze its frequency components to understand the heart's activity better.

The key frequencies in the HRV signal are typically:

**Low Frequency (LF):** Associated with sympathetic nervous activity.

**High Frequency (HF):** Associated with parasympathetic nervous activity.
Very Low Frequency (VLF): Associated with long-term rhythms like thermoregulation.


We can use FFT to extract the frequency components of HRV.
Clean the signal and remove high-frequency noise.
Identify the LF and HF components and analyze the balance between them

<img width="636" height="391" alt="image" src="https://github.com/user-attachments/assets/368c84a8-982e-47f3-ad5a-c28e9632bce6" />

<img width="651" height="400" alt="image" src="https://github.com/user-attachments/assets/dad1f56b-d4d3-475d-a530-42204498336d" />


## Technical Conclusion & Analysis

This project successfully demonstrates the application of **Discrete Fourier Transform (DFT)** and **digital frequency filtering** to isolate harmonic components from stochastic noise. By synthesizing a multi-component signal ($f_1=50\text{Hz}$, $f_2=120\text{Hz}$) and a 5Hz Simple Harmonic Oscillator sampled at $F_s=1000\text{Hz}$, we established a high-fidelity baseline for spectral analysis. 

### Key Parameters & Results:
*   **Sampling Frequency ($F_s$):** $1000\text{ Hz}$
*   **HRV Filter Passband:** $[0.05, 0.3]\text{ Hz}$
*   **Mechanical Cutoff:** $6\text{ Hz}$ (Low-Pass)

The implementation proved effective in recovering underlying physical motion. A critical technical milestone was achieving **Hermitian symmetry** within the FFT array—by capturing both positive and negative frequency peaks via `np.abs(f)`—ensuring that the reconstructed time-domain signal maintained 100% of its original amplitude and phase integrity. The results validate that frequency-domain attenuation of high-frequency AWGN (Additive White Gaussian Noise) is a robust method for extracting precise mechanical and physiological parameters from corrupted sensor data.




