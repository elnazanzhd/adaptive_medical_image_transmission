
# Adaptive Transmission of Medical Images over Bandwidth-Limited Networks

## Overview
This project investigates **communication-aware transmission of medical images** under bandwidth constraints and unreliable channels.
The focus is on **rate–distortion behavior** and **robustness to channel impairments** when using **learned compression** for
telemedicine-oriented systems.

The project integrates:
- **Communications engineering** (bitrate constraints, packet loss, channel noise)
- **Machine learning** (learned image compression with autoencoders)
- **Medical informatics** (diagnostic image transmission)

---

## Dataset
We use **OrganAMNIST** from **MedMNIST v2**, a professionally curated medical imaging benchmark hosted on **Zenodo** and widely used in academic research.

- Modality: abdominal organ medical images  
- Image size: 28 × 28 (grayscale)  
- Motivation: clean anatomical structure, reproducibility, and suitability for communication-focused experiments  

A **subset of the dataset** was used to keep experiments computationally efficient while preserving system-level trends.

---

## System Architecture

```
Medical Image
   ↓
Encoder (CNN)
   ↓
Latent Representation
   ↓
Quantization (bit-depth control)
   ↓
Simulated Communication Channel
   ↓
Decoder (CNN)
   ↓
Reconstructed Image
```

### Channel Model
The communication channel is simulated in latent space and includes:
- **Bandwidth limitation** via latent quantization (2–8 bits)
- **Packet loss** (5% and 10%)
- **Additive noise** (latent SNR = 20 dB and 10 dB)

---

## Learned Compression Model
A lightweight **convolutional autoencoder** is used:
- Encoder compresses the image into a compact latent representation
- Decoder reconstructs the image from the (possibly corrupted) latent
- Training objective: mean squared error (MSE)

### Training Dynamics
The model was trained for **10 epochs**. Training and validation losses decreased smoothly and remained closely aligned,
indicating stable optimization and no significant overfitting.

---

## Rate–Distortion Results (Clean Channel)

### PSNR vs Bitrate
![PSNR vs bpp](figures/rd_psnr.png)

### SSIM vs Bitrate
![SSIM vs bpp](figures/rd_ssim.png)

**Findings:**
- Increasing bitrate from **2 → 3 bpp** yields a significant improvement.
- Beyond **3–4 bpp**, both PSNR and SSIM saturate.
- Additional bitrate up to 8 bpp provides negligible gains.

**Interpretation:** The learned model achieves a compact latent representation and becomes **representation-limited** rather than bitrate-limited.

---

## Qualitative Reconstruction
![Original vs Reconstruction](figures/original_vs_reconstruction.png)

Visual inspection confirms that anatomical structures are preserved at moderate bitrates, and higher bitrates do not
produce meaningful visual improvements.

---

## Robustness to Packet Loss
![Robustness to Packet Loss](figures/robustness_packet_loss.png)

**Observations:**
- Packet loss causes a consistent PSNR degradation across all bitrates.
- Increasing bitrate beyond 3–4 bpp does not recover lost quality.
- Channel reliability dominates performance under lossy conditions.

---

## Robustness to Latent Noise
![Robustness to Latent Noise](figures/robustness_latent_noise.png)

**Observations:**
- Latent noise causes graceful degradation.
- Higher bitrates offer limited robustness gains.
- Trends remain consistent across operating points.

---

## System-Level Insights
1. Rate–distortion optimality does not imply robustness.
2. Bandwidth efficiency saturates early in learned compression.
3. Channel impairments often dominate system performance.
4. Communication-aware ML design is critical for medical systems.

---


