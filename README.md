Coverless Steganography Using Stable Diffusion
Introduction
Steganography is the technique of hiding secret information inside an ordinary-looking file, such as an image, audio clip, or video. Unlike cryptography, which protects the content of a message by making it unreadable, steganography aims to conceal the very existence of the message.

In traditional image steganography, information is embedded by modifying an existing cover image. These modifications can introduce small changes in pixel values, which may be detected by steganalysis techniques.

Coverless steganography takes a different approach. Instead of depending on a pre-existing cover image, it uses image selection, transformation, or generation techniques to create or identify a carrier that represents the secret information. The goal is to make covert communication less noticeable while preserving the visual appearance of the carrier.
About This Project
In this project, I explore a coverless image steganography approach that uses Stable Diffusion to generate a carrier image and pixel-level processing to embed secret information.

The workflow starts by generating an image from a text prompt. The secret message is converted into binary data, and the generated image is processed in 3×3 pixel blocks. In the described embedding method, selected pixel values in the red channel are compared using a parity calculation. If the calculated parity does not match the message bit, a small pixel-value adjustment is made.

At the receiving end, the same pixel positions and parity calculation are used to extract the hidden bitstream. The recovered data is then converted back into its original representation.

The project also discusses Integer Wavelet Transform (IWT), image-channel processing, Base64 representation, and image-quality evaluation using PSNR and SSIM.
Terminology note: The publication describes the work as coverless steganography. Its detailed pixel-embedding procedure also makes small changes to selected pixels in the generated carrier image. I retain the terminology used in the paper while describing that implementation detail transparently.

How It Works
1. Generate a carrier image
A text prompt is provided to Stable Diffusion to generate a natural-looking image. The paper uses the prompt "A running horse" as an example and describes a 768 × 768 carrier image.
2. Prepare the secret data
The secret message is converted into a binary sequence. For secret-image data, the described workflow also uses Base64 representation so the image bytes can be represented as text before conversion to binary.
3. Embed the secret data
The generated image is separated into its RGB channels. The red channel is traversed in 3×3 pixel blocks.

For each block, two pixel positions are selected:
p1 = R[j + 2, i]
p2 = R[j + 1, i + 1]
The bit represented by the pixel pair is calculated using:
bit = abs(p1 - p2) % 2
- An even absolute difference represents 0.
- An odd absolute difference represents 1.
- If the calculated bit differs from the current secret bit, one selected pixel is adjusted by +1 or -1 to obtain the required parity.
After processing, the RGB channels are merged to create the stego-image.
4. Extract the secret data
The receiver loads the stego-image and separates its RGB channels. The same 3×3 block traversal and pixel positions are used to calculate the parity bits.

The extracted bits are grouped into bytes and converted back into the message representation. For secret-image data, the Base64 content is decoded into image bytes and reconstructed as an image.
5. Evaluate the output
The publication evaluates the method using:
- Embedding efficiency (bpp): amount of embedded data relative to image size.
- Data recovery precision: proportion of correctly recovered bits.
- PSNR: measures pixel-level image distortion.
- SSIM: measures structural similarity between images.
Technologies and Concepts
- Python – implementation environment
- Stable Diffusion – carrier image generation
- OpenCV – image loading, channel processing, and reconstruction
- PyWavelets – wavelet-transform operations
- Integer Wavelet Transform (IWT) – image-domain processing discussed in the method
- Base64 – representation of secret image bytes
- Google Colab with GPU support – environment described for implementation
Results Reported in the Publication
The following values are taken from the paper and are included here as reported results. They have not been independently reproduced in this repository.
Image dimension	Embedding level	PSNR	SSIM
768 × 768	100%	51.61 dB	0.99
768 × 768	80%	50.94 dB	0.99
768 × 768	50%	47.70 dB	0.99
768 × 768	20%	46.52 dB	0.99
768 × 768	1%	47.25 dB	0.99
1000 × 1000	100%	34.95 dB	0.97
2000 × 2000	100%	40.33 dB	0.98


The paper also lists the following payload capacities:
Image dimension	Payload capacity
768 × 768	1,769,472 bits
1000 × 1000	3,000,000 bits
2000 × 2000	12,000,000 bits


Repository Structure
coverless-steganography-stable-diffusion/
├── README.md
├── .gitignore
├── requirements.txt
├── docs/
│   └── paper-reference.md
└── src/
    └── README.md
Setup
This repository currently contains the project explanation and research reference. The executable implementation and a tested dependency lockfile have not yet been added.

To prepare a local Python environment:
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate

pip install -r requirements.txt
Stable Diffusion setup may require additional model configuration, hardware resources, and version-specific dependencies.
Notes and Limitations
- The metrics in this README are reported in the publication; they are not independent benchmark results from this repository.
- The method's robustness against resizing, compression, cropping, filtering, and other image transformations requires further testing.
- Steganography and encryption solve different problems. Sensitive payloads should be encrypted with a vetted cryptographic library before embedding.
- Small visual changes do not by themselves prove that an image is undetectable by modern steganalysis.
- The detailed embedding procedure described in the paper adjusts pixel values, even though the overall approach is presented as coverless steganography.
Publication
This project is based on the following IEEE conference publication:

“Enhancing CyberSecurity Through Coverless Steganography with Diffusion Based Image Synthesis”
- IEEE Xplore: https://ieeexplore.ieee.org/document/11484830
- Publication record: View on IEEE Xplore
Please refer to the IEEE Xplore record for the definitive bibliographic metadata, including the conference details, publication year, page range, and DOI.
License
No open-source license has been selected for this repository. Unless a license is added, the source and documentation remain subject to their applicable copyright terms.
