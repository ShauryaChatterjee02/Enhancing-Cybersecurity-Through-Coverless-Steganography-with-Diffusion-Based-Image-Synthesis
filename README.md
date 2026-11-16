
# Coverless Steganography Using Stable Diffusion

## Overview

Steganography is the technique of hiding confidential information inside seemingly harmless digital media such as images, audio, text, and videos. Unlike cryptography, which protects information by converting it into an unreadable format, steganography focuses on concealing the existence of the secret information itself.

Traditional image steganography embeds secret data into existing cover images by modifying pixel values. However, these modifications can introduce detectable characteristics that advanced steganalysis techniques may identify.

**Coverless Steganography** introduces an alternative approach that uses image selection, transformation, or generation to create carriers that inherently represent secret information, reducing dependence on conventional modifications to existing media.

This project explores an AI-based approach to coverless steganography by combining **Stable Diffusion, Integer Wavelet Transform (IWT), and parity-based pixel embedding** for secure covert communication.

## About the Project

In this project, I explore the integration of generative AI and image steganography to develop a secure image-based communication approach.

The proposed methodology uses Stable Diffusion to generate high-quality carrier images from textual prompts. Secret information is converted into binary data and embedded into the generated image using a parity-based technique.

The generated image is divided into 3×3 pixel blocks. Selected pixel positions in the red channel are used to calculate parity values, which represent the binary information being embedded.

The embedding process uses small pixel-value adjustments to match the required parity while maintaining the visual quality of the generated image.

Integer Wavelet Transform (IWT) is also incorporated into the image-processing workflow. At the receiving end, the embedded information is extracted using the corresponding pixel positions and parity calculations.

The recovered binary information is converted back into its original representation, allowing the reconstruction of the hidden message or image.

## Key Features

- AI-based carrier image generation using Stable Diffusion.
- Coverless image steganography approach.
- 3×3 pixel-block-based data embedding.
- Red-channel parity-based encoding.
- Integer Wavelet Transform (IWT).
- Binary data conversion and extraction.
- Base64 encoding and decoding.
- Secret image reconstruction using OpenCV.
- Image quality evaluation using PSNR and SSIM.
- Data recovery precision analysis.

## Methodology

The proposed methodology combines three core strategies:

### 1. Generative Diversity

Stable Diffusion is used to generate high-quality carrier images from textual descriptions.

The generated images provide natural textures and visual diversity, helping reduce the risk of pattern detection.

Example prompt:

```text
A running horse
```

The model transforms random noise into a semantically coherent 768 × 768 RGB image.

### 2. Parity-Based Embedding

Secret information is embedded into the red channel using a modulo-2 parity technique.

The image is divided into 3×3 pixel blocks, and two selected pixel positions are used to calculate the parity value.

The selected pixel positions are:

```python
p1 = R[j + 2, i]
p2 = R[j + 1, i + 1]
```

The parity calculation is:

```python
bit = abs(p1 - p2) % 2
```

The parity represents the binary information:

- Even difference → 0
- Odd difference → 1

If the calculated parity does not match the target secret bit, the first selected pixel is incremented or decremented to obtain the required parity.

### 3. Data Integrity

The extraction process reverses the embedding procedure to recover the hidden information.

The same pixel positions and parity calculations are used to retrieve the binary sequence.

The recovered information is converted into its original representation, with the objective of maintaining data recovery accuracy and minimizing image distortion.

## Project Workflow

### Step 1: Image Generation Using Stable Diffusion

A textual prompt is provided to Stable Diffusion.

The model generates a 768 × 768 RGB carrier image that serves as the host image for secret-data embedding.

### Step 2: Secret Data Preprocessing

The secret message or image is converted into binary data.

The preprocessing workflow includes:

- Reading the secret information.
- Converting the message into binary representation.
- Converting secret-image data into Base64 representation.
- Preparing the binary sequence for embedding.

### Step 3: RGB Channel Separation

The generated carrier image is separated into its Blue, Green, and Red channels.

The embedding operation is performed on the red channel.

### Step 4: Data Embedding

The red channel is divided into 3×3 pixel blocks.

For each block:

1. Select two predefined pixel positions.
2. Calculate their absolute difference.
3. Compute the modulo-2 parity.
4. Compare the calculated parity with the target message bit.
5. Adjust the first selected pixel if required.

The modified red channel is merged with the original blue and green channels to generate the stego-image.

### Step 5: Secret Data Extraction

The receiver loads the stego-image and separates its RGB channels.

The red channel is traversed using the same 3×3 block structure.

The parity values are calculated to recover the embedded binary information.

### Step 6: Secret Image Reconstruction

The extracted binary sequence is grouped into 8-bit segments.

The recovered information is converted into its corresponding Base64 representation.

The Base64 data is decoded into image bytes, and OpenCV is used to reconstruct the original secret image.

## Technologies and Tools

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Stable Diffusion | AI-based carrier image generation |
| OpenCV | Image processing and reconstruction |
| PyWavelets | Wavelet transformations |
| Integer Wavelet Transform (IWT) | Image-domain processing |
| NumPy | Numerical and array operations |
| Base64 | Secret image data representation |
| scikit-image | Image quality evaluation |
| Google Colab | Development environment |
| GPU | Accelerated image generation and processing |

## Experimental Results and Analysis

The proposed approach is evaluated using different image dimensions and embedding percentages.

The performance analysis considers three major aspects:

### 1. Data Embedding Efficiency

Embedding efficiency, measured in bits per pixel (bpp), represents the amount of secret information embedded relative to the cover image dimensions.

The paper reports embedding efficiency ranging from:

- 0.215 bpp at 128 × 128 resolution.
- 0.222 bpp at 1024 × 1024 resolution and above.

### 2. Data Recovery Precision

Data recovery precision measures the percentage of correctly extracted bits compared with the total number of embedded bits.

The calculation is:

```text
Data Recovery Precision (%) =

(Number of Correct Bits / Total Number of Bits) × 100
```

### 3. Visual Integrity

Image quality is evaluated using:

- **PSNR (Peak Signal-to-Noise Ratio):** Measures image distortion.
- **SSIM (Structural Similarity Index):** Measures structural similarity between images.

The paper reports the following visual integrity observations:

| Metric | Reported Value |
|---|---:|
| AI-generated image PSNR | 83.72 dB |
| Stego-image PSNR | 61.92 dB |
| AI-generated image SSIM | 1.0000 |
| Stego-image SSIM | 0.9999 |
| AI-generated image size | 505.80 KB |
| Stego-image size | 53.92 KB |

## Performance Metrics

The following results are reported in the IEEE publication.

| Image Dimension | Embedding (%) | PSNR (dB) | SSIM |
|---|---:|---:|---:|
| 768 × 768 | 100 | 51.61 | 0.99 |
| 768 × 768 | 80 | 50.94 | 0.99 |
| 768 × 768 | 50 | 47.70 | 0.99 |
| 768 × 768 | 20 | 46.52 | 0.99 |
| 768 × 768 | 1 | 47.25 | 0.99 |
| 1000 × 1000 | 100 | 34.95 | 0.97 |
| 1000 × 1000 | 80 | 35.96 | 0.97 |
| 1000 × 1000 | 50 | 30.94 | 0.99 |
| 1000 × 1000 | 20 | 30.91 | 0.99 |
| 1000 × 1000 | 1 | 33.32 | 0.99 |
| 2000 × 2000 | 100 | 40.33 | 0.98 |
| 2000 × 2000 | 80 | 37.34 | 0.98 |
| 2000 × 2000 | 50 | 37.21 | 0.98 |
| 2000 × 2000 | 20 | 34.21 | 0.98 |
| 2000 × 2000 | 1 | 47.25 | 0.98 |

### Payload Capacity

| Image Dimension | Payload Capacity (Bits) |
|---|---:|
| 768 × 768 | 1,769,472 |
| 1000 × 1000 | 3,000,000 |
| 2000 × 2000 | 12,000,000 |

*Note: All experimental values are taken from the published paper and have not been independently reproduced in this repository.*

## Applications

- Secure digital communication.
- Confidential information transmission.
- Image-based covert communication.
- Cybersecurity and information protection.
- AI-driven steganography research.
- Digital forensic research.

## Future Scope

The research identifies several opportunities for further development:

- Developing adaptive diffusion architectures.
- Improving resistance against advanced steganalysis.
- Integrating adversarial training.
- Exploring transformer-based latent diffusion models.
- Integrating cryptographic key conditioning.
- Conducting formal security proofs.
- Performing information-theoretic capacity analysis.
- Exploring hybrid watermarking and steganography.
- Developing real-time deployment frameworks for cloud and edge environments.

## Publication

This project is based on the following IEEE conference publication:

**Enhancing CyberSecurity Through Coverless Steganography with Diffusion Based Image Synthesis**

**Conference:** 2026 Innovations in Machine, Engineering, and Digital Conference (IMED)

**Publisher:** IEEE

**DOI:** 10.1109/IMED68921.2026.11484830

**IEEE Xplore:**  
https://ieeexplore.ieee.org/document/11484830

**DOI Link:**  
https://doi.org/10.1109/IMED68921.2026.11484830

## Repository Structure

```text
Coverless-Steganography-Stable-Diffusion/
│
├── README.md
│
├── src/
│   ├── image_generation.py
│   ├── preprocessing.py
│   ├── embedding.py
│   ├── extraction.py
│   └── evaluation.py
│
├── notebooks/
│   └── steganography_implementation.ipynb
│
├── results/
│   ├── cover_image/
│   ├── stego_image/
│   └── extracted_image/
│
├── docs/
│   └── research_paper.pdf
│
└── requirements.txt
```

*The above is a suggested repository organization. The actual executable source files should be added separately.*

## Conclusion

This project explores the integration of generative AI and image steganography for secure covert communication.

By combining Stable Diffusion-based image synthesis, 3×3 pixel-block parity embedding, Integer Wavelet Transform, and image-processing techniques, the proposed approach investigates a method for embedding and extracting confidential information from digital images.

The experimental evaluation using PSNR, SSIM, embedding efficiency, and payload capacity demonstrates the image-quality characteristics reported in the research.

The work provides a foundation for further research into diffusion-based image synthesis, AI-driven steganography, and cybersecurity applications.

---

**Research Areas:** Generative AI | Cybersecurity | Image Processing | Computer Vision | Steganography

**Publication:** [IEEE Xplore](https://ieeexplore.ieee.org/document/11484830)
