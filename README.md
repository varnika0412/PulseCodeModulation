# Reg.no : 212224060292
# Name   : VARNIKA K
# Pulse-Code-Modulation and Delta Modulation
# Aim
Write a simple Python program for the modulation and demodulation of PCM, and DM.
# Tools required
 * computer with google colab
# Program
```
import numpy as np
import matplotlib.pyplot as plt
# ===== Signal Parameters =====
frequency = 2 # Hz
amplitude = 1
duration = 2 # seconds
analog_rate = 1000 # Hz for smooth analog plot
sample_rate = 10 # Hz sampling
num_levels = 8 # Quantization levels
# ===== Analog Signal =====
t = np.linspace(0, duration, int(analog_rate * duration), endpoint=False)
analog_signal = amplitude * np.sin(2 * np.pi * frequency * t)
plt.figure(figsize=(8,4))
plt.plot(t, analog_signal)
plt.title("Original Analog Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== Sampling =====
t_samp = np.linspace(0, duration, int(sample_rate * duration), endpoint=False)
sampled_signal = amplitude * np.sin(2 * np.pi * frequency * t_samp)
plt.figure(figsize=(8,4))
plt.stem(t_samp, sampled_signal, linefmt='r-', markerfmt='ro', basefmt=' ')
plt.title("Sampled Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== Quantization =====
max_amp = np.max(np.abs(sampled_signal))
step = 2 * max_amp / num_levels
indices = np.round((sampled_signal + max_amp) / step)
indices = np.clip(indices, 0, num_levels-1).astype(int)
quantized_signal = (indices * step) - max_amp
plt.figure(figsize=(8,4))
plt.step(t_samp, quantized_signal, where='mid', color='g')
plt.title("Quantized Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== PCM Encoding (Binary) =====
num_bits = int(np.log2(num_levels))
binary_codes = [np.binary_repr(idx, width=num_bits) for idx in indices]
print("Binary Codes for Samples:", binary_codes)
# ===== Transmission (simulated) =====
# For demonstration, we assume noiseless channel
transmitted_codes = binary_codes.copy()
# ===== PCM Decoding =====
decoded_indices = np.array([int(code, 2) for code in transmitted_codes])
decoded_signal = (decoded_indices * step) - max_amp
plt.figure(figsize=(8,4))
plt.step(t_samp, decoded_signal, where='mid', color='purple')
plt.stem(t_samp, decoded_signal, linefmt='g:', markerfmt='go', basefmt=' ')
plt.title("Reconstructed PCM Signal (Demodulated)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()
# ===== Optional: Compare Original Analog and Reconstructed Signal =====
plt.figure(figsize=(8,4))
plt.plot(t, analog_signal, label='Original Analog', alpha=0.5)
plt.step(t_samp, decoded_signal, where='mid', color='purple', label='Reconstructed PCM')
plt.title("Original vs Reconstructed PCM Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()
plt.show()
```

# Delta Modulation
```
import numpy as np
import matplotlib.pyplot as plt

# ===== Signal Parameters =====
frequency = 2        # Hz
amplitude = 1
duration = 2         # seconds
analog_rate = 1000   # Hz for smooth analog plot
sample_rate = 50     # Hz sampling (higher than PCM for better DM)
delta = 0.2          # Step size (important parameter in DM)

# ===== Analog Signal =====
t = np.linspace(0, duration, int(analog_rate * duration), endpoint=False)
analog_signal = amplitude * np.sin(2 * np.pi * frequency * t)

plt.figure(figsize=(8,4))
plt.plot(t, analog_signal)
plt.title("Original Analog Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()

# ===== Sampling =====
t_samp = np.linspace(0, duration, int(sample_rate * duration), endpoint=False)
sampled_signal = amplitude * np.sin(2 * np.pi * frequency * t_samp)

plt.figure(figsize=(8,4))
plt.stem(t_samp, sampled_signal, linefmt='r-', markerfmt='ro', basefmt=' ')
plt.title("Sampled Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()

# ===== Delta Modulation Encoding =====
dm_output = []
reconstructed = []

prev_value = 0   # Initial predicted value

for sample in sampled_signal:
    if sample >= prev_value:
        dm_output.append(1)
        prev_value += delta
    else:
        dm_output.append(0)
        prev_value -= delta
    reconstructed.append(prev_value)

dm_output = np.array(dm_output)
reconstructed = np.array(reconstructed)

print("Delta Modulated Bit Stream:")
print(dm_output)

# ===== Plot Reconstructed Signal =====
plt.figure(figsize=(8,4))
plt.step(t_samp, reconstructed, where='mid', color='purple')
plt.title("Reconstructed Signal (Delta Demodulation)")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.show()

# ===== Comparison Plot =====
plt.figure(figsize=(8,4))
plt.plot(t, analog_signal, label='Original Analog', alpha=0.5)
plt.step(t_samp, reconstructed, where='mid', color='purple', label='Reconstructed DM')
plt.title("Original vs Reconstructed Delta Modulated Signal")
plt.xlabel("Time (s)")
plt.ylabel("Amplitude")
plt.grid(True)
plt.legend()
plt.show()
```

# Output Waveform
# Pulse-Code-Modulation Waveform
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/e88c82a0-74d1-45b1-aa38-d9212b305717" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/0222a369-4fc1-40d8-a018-9d9b4acaf208" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/cf68ea0f-c1ed-4101-af0f-fe69538af01b" />



```
Binary Codes for Samples: ['100', '111', '110', '010', '000', '100', '111', '110', '010', '000', '100', '111', '110', '010', '000', '100', '111', '110', '010', '000']
```


<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/6597b3f0-0606-4043-9387-c0131c8b2101" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/7a39bd79-f047-44e4-92f6-92739fc08282" />

# Delta Modulatio Waveform
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/98952a4e-5f68-4d44-a257-49eceb248104" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/9c0162f3-bc73-4f36-a7ff-4be0a3bea2e0" />



```
Delta Modulated Bit Stream:
[1 1 1 1 1 0 1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 1 1 1 1 1 1 1 1 1 1 0 1 0 0 0 0
 0 0 0 0 0 0 1 0 1 0 1 1 1 1 1 1 1 1 1 1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 1 1 1
 1 1 1 1 1 1 1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 1 0 1 1 1]
```


<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/afda6acf-4158-4493-8b0c-17ce854c6396" />
<img width="711" height="393" alt="image" src="https://github.com/user-attachments/assets/b82b45b2-fec8-4b89-96c0-c282cf57b692" />


# Results
The analog signal was successfully encoded and reconstructed using PCM and DM techniques in Python, verifying their working principles.

