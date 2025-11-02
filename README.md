```markdown
# Video Steganography using AES

Embed encrypted secret data (images, text, small files) into a video using steganography techniques combined with AES symmetric encryption. This repository demonstrates a simple pipeline to securely hide information inside video frames and recover it later with the correct key.

> Note: This README gives usage examples and guidance. Update the command names/flags below to match the actual scripts in this repository if they differ.

## Features

- AES encrypts secret data before embedding for confidentiality.
- Embeds encrypted payload into video frames (e.g., via least-significant-bit techniques).
- Extracts and decrypts payload with the correct key.
- Minimal dependencies; implemented in Python for portability and ease of experimentation.

## How it works (high level)

1. The secret payload (an image, text file, or any small file) is read and encrypted using AES (recommended: AES-256).
2. The encrypted bytes are split across frames of the carrier video and written into pixel LSBs or another embedding channel.
3. To retrieve the payload, frames are read from the stego-video, encrypted bytes are recomposed, and AES decryption is applied using the original key.
4. If the provided key is incorrect or the video was altered, decryption will fail or produce garbage.

## Security notes

- The security of the hidden data depends on both:
  - The strength and secrecy of the AES key/passphrase.
  - The secrecy of the embedding method and the carrier video.
- Always use a sufficiently long, high-entropy key (e.g., a randomly generated 32-byte key for AES-256). If the implementation derives a key from a passphrase (e.g., via PBKDF2 or SHA-256), prefer using a long passphrase.
- This project is for education and experimentation. Do not use it for protecting highly sensitive data without a security review.

## Requirements

- Python 3.8+
- Common libraries (install with pip):
  - opencv-python
  - pycryptodome (or cryptography, depending on implementation)
  - numpy
  - pillow (PIL) — optional, for image handling

Install requirements:
```bash
pip install opencv-python pycryptodome numpy pillow
```
Or, if there is a requirements.txt:
```bash
pip install -r requirements.txt
```

## Typical usage

Adjust filenames and script names to match what's in this repo.

Embed (encrypt + hide):
```bash
python embed.py \
  --input-video carrier.mp4 \
  --secret secret.png \
  --output-video stego.mp4 \
  --passphrase "my strong passphrase"
```

Extract (read + decrypt):
```bash
python extract.py \
  --stego-video stego.mp4 \
  --output-file recovered_secret.png \
  --passphrase "my strong passphrase"
```

Example notes:
- If the implementation requires a raw AES key, provide a 32-byte hex or base64 key for AES-256.
- If a passphrase is used, the code may derive a key (e.g., SHA-256 or PBKDF2). Use the same passphrase for embedding and extraction.

## Limits & capacity

- Embedding capacity depends on:
  - Number of frames in the carrier video.
  - Resolution and number of color channels used for embedding.
  - How many LSBs per channel you modify (1 LSB per channel is safest for stealth).
- Larger payloads may require higher-capacity carrier videos or modifying more bits (which increases detectability and degrades video quality).

## Tips for better stealth & reliability

- Use longer carrier videos or higher-resolution videos for larger payloads.
- Avoid recompression or lossy processing of the stego-video after embedding — lossy encoders (re-encoding with high compression) can destroy hidden data.
- Test round-trip (embed -> extract) on sample video to verify parameters before using real data.

## Development & contribution

- If you'd like to contribute, please:
  - Open an issue describing the feature or bug.
  - Submit a pull request with tests and clear commit messages.
- Keep changes small and focused.

## Troubleshooting

- Extraction fails or produces garbage:
  - Ensure the exact same passphrase/key is used.
  - Ensure the stego-video was not re-encoded or altered after embedding.
- Script errors about missing modules:
  - Install required packages (see Requirements section).

## License

See the LICENSE file in this repository for license details. If there is no LICENSE file yet, add one (MIT is common for example projects).

## Contact

Maintainer: @AbondentSpace1386

If you want the README to include:
- exact CLI flags for the repository scripts,
- example screenshots or a demo video,
- a short technical section about the exact embedding algorithm used (LSB, frequency domain, etc.),

tell me which scripts/filenames exist in the repo or paste the main script headers and I will update the README to match precisely.
```
