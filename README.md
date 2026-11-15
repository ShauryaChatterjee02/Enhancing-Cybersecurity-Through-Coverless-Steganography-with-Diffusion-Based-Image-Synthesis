
# Coverless Steganography Using Stable Diffusion

## Introduction

Steganography is the technique of hiding secret information inside ordinary digital files such as images, audio, and videos. Unlike cryptography, which converts information into an unreadable format, steganography focuses on hiding the existence of the secret message itself.

Traditional image steganography modifies existing images to embed confidential information. These modifications may introduce detectable changes in pixel values, which can potentially be identified using steganalysis techniques.

**Coverless Steganography** introduces an alternative approach in which secret information is represented through image selection, transformation, or generation rather than relying exclusively on modifying an existing cover image. This approach aims to make hidden communication less noticeable.

## About the Project

In this project, I explore an AI-based coverless image steganography approach using **Stable Diffusion, Integer Wavelet Transform (IWT), and pixel-based parity encoding** to achieve secure covert communication.

The proposed approach generates a carrier image using Stable Diffusion and embeds secret information through image-processing techniques.

The generated image is divided into 3×3 pixel blocks. Selected pixel values from the red channel are compared using a parity-based encoding mechanism to represent binary data.

If the calculated parity does not match the corresponding secret message bit, a small adjustment is made to the selected pixel value.

At the receiver's end, the same pixel positions and parity calculations are used to extract the hidden binary information and reconstruct the original secret data.

The project also incorporates encryption, Base64 encoding, image processing, and image-quality evaluation to examine the effectiveness of the proposed approach.

> **Note:** Although the research is presented as coverless steganography, the described pixel-level embedding procedure makes small modifications to selected pixels of the generated carrier image.

## Key Features

- AI-generated carrier images using Stable Diffusion.
- Coverless steganography-based communication approach.
- Secret message conversion into binary format.
- 3×3 pixel block-based data embedding.
- Pixel-difference parity encoding.
- Integer Wavelet Transform (IWT) for image processing.
- Base64 encoding and decoding.
- Secret data extraction and reconstruction.
- Image-quality evaluation using PSNR and SSIM.
- Secure communication using encryption techniques.

## Project Workflow

### 1. Carrier Image Generation

Stable Diffusion generates a natural-looking image using a user-defined text prompt.

Example prompt:

```text
A running horse
```

The generated image is used as the carrier image for the embedding process.

### 2. Secret Data Preprocessing

The secret message or image is converted into a suitable binary representation.

The preprocessing stage involves:

- Reading the secret information.
- Applying encryption.
- Converting image data into Base64 representation when applicable.
- Converting the resulting data into binary format.

### 3. Image Processing and Data Embedding

The generated carrier image is separated into RGB channels.

The red channel is divided into 3×3 pixel blocks.

For every block, two pixel positions are selected:

```python
p1 = R[j + 2, i]
p2 = R[j + 1, i + 1]
```

The absolute difference between these pixel values is calculated.

**Parity Calculation:**

```python
bit = abs(p1 - p2) % 2
```

The parity represents the binary information:

- Even difference → 0
- Odd difference → 1

If the calculated parity differs from the secret message bit, one selected pixel value is adjusted by +1 or -1.

This process continues until the secret binary data is embedded.

### 4. Stego Image Generation

After embedding the secret information, the modified RGB channels are merged to generate the final stego-image.

The generated image is saved and transmitted to the receiver.

### 5. Secret Data Extraction

The receiver processes the stego-image using the same extraction mechanism.

The extraction process involves:

1. Loading the stego-image.
2. Separating RGB channels.
3. Extracting the red channel.
4. Dividing the image into 3×3 pixel blocks.
5. Calculating pixel-difference parity.
6. Recovering the binary sequence.
7. Converting binary data into its original representation.
8. Decoding Base64 data when applicable.
9. Reconstructing the original secret image or message.

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Stable Diffusion | AI-based carrier image generation |
| OpenCV | Image processing and manipulation |
| PyWavelets | Wavelet transformations |
| Integer Wavelet Transform | Image-domain processing |
| NumPy | Numerical and array operations |
| Base64 | Binary data representation |
| Fernet Encryption | Secret data encryption |
| Google Colab | Development and GPU execution |
| Matplotlib | Image visualization |

## Experimental Results

The proposed approach was evaluated using image-quality metrics such as Peak Signal-to-Noise Ratio (PSNR) and Structural Similarity Index (SSIM).

### PSNR and SSIM Results

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
| 2000 × 2000 | 100 | 40.33 | 0.98 |

### Payload Capacity

| Image Dimension | Payload Capacity |
|---|---:|
| 768 × 768 | 1,769,472 bits |
| 1000 × 1000 | 3,000,000 bits |
| 2000 × 2000 | 12,000,000 bits |

*Note: These values are taken from the published research paper and have not been independently reproduced in this repository.*

## Applications

- Secure communication.
- Confidential data transmission.
- Digital information protection.
- Image-based covert communication.
- Research in AI-driven steganography.
- Cybersecurity and information hiding.

## Limitations and Future Scope

Although the proposed approach demonstrates promising image-quality results, several areas can be explored further:

- Improving resistance against advanced steganalysis.
- Increasing embedding capacity.
- Improving extraction accuracy.
- Testing robustness against image compression and resizing.
- Exploring advanced diffusion models.
- Optimizing computational efficiency.
- Improving encryption and authentication mechanisms.

## Publication

This project is associated with the following IEEE conference publication:

**Enhancing CyberSecurity Through Coverless Steganography with Diffusion Based Image Synthesis**

Published research paper:

[Read the Research Paper on IEEE Xplore](https://ieeexplore.ieee.org/document/11484830)

The publication presents the proposed methodology, implementation workflow, algorithms, and experimental evaluation of the research.

## Repository Structure

```text
Coverless-Steganography-Stable-Diffusion/
│
├── README.md
│
├── src/
│   ├── image_generation.py
│   ├── preprocessing.py
│   ├── encryption.py
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

*The structure above is a suggested organization for the implementation.*

## Conclusion

This project explores the combination of generative AI and image steganography for secure covert communication.

By integrating Stable Diffusion-based image generation with pixel-level parity encoding, Integer Wavelet Transform, and encryption techniques, the proposed approach investigates an alternative way of embedding and recovering confidential information within digital images.

The research highlights the potential of generative AI in cybersecurity and provides a foundation for further exploration of AI-driven steganography techniques.

---

**Research Area:** Artificial Intelligence | Cybersecurity | Image Processing | Steganography | Generative AI

**Publication:** [IEEE Xplore](https://ieeexplore.ieee.org/document/11484830)
