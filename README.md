# 🎬 Chaotic Video Watermarking using Henon Map, DWT & SVD


<img width="1672" height="941" alt="cover" src="https://github.com/user-attachments/assets/18df2cb1-8544-42a9-87e2-0011321beed7" />


<div align="center">

![TÜBİTAK](https://img.shields.io/badge/TÜBİTAK-2209--A%20Funded-red?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=for-the-badge&logo=mathworks&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

**Kırıkkale University — Computer Engineering Department**
**2024–2025 Graduation Project | TÜBİTAK 2209-A Research Support**

</div>

---

## 📌 Overview

This project presents a novel **digital video watermarking method** that combines three powerful techniques:

- **Henon Chaotic Map** — for unpredictable, key-based pixel permutation
- **DWT** (Discrete Wavelet Transform / Ayrık Dalgacık Dönüşümü) — for frequency-domain embedding
- **SVD** (Singular Value Decomposition / Tekil Değer Ayrışımı) — for robust watermark merging

The result is a watermarking system that is simultaneously **imperceptible to viewers** and **robust against attacks** such as noise addition, compression, cropping, and blurring.

> 🏆 This research was awarded funding under the **TÜBİTAK 2209-A University Students Research Projects Support Program** and was presented at the **Kırıkkale University Computer Engineering 2024–2025 Graduation Projects Exhibition**.

---

## 👥 Team

| Role | Name |
|------|------|
| 🎓 Advisor | Assoc. Prof. Dr. Fahrettin HORASAN |
| 👨‍💻 Developer | Yasir Ayad Hamed AL RAWEE |
| 👨‍💻 Developer | Yusuf Ömer KARA |

---

## 📊 Results

| Metric | Value | Description |
|--------|-------|-------------|
| **PSNR** | 28.61 dB | Visual quality — watermark imperceptible |
| **SSIM** | 0.96 | Structural similarity near-identical to original |
| **NC** | 0.93 | Watermark recovery under attack conditions |

---

## 🔬 How It Works

### Embedding Pipeline

```
Video Frame
    │
    ▼
┌─────────┐     ┌─────────┐
│   DWT   │     │ Watermark│
│  (ADD)  │     │  Image  │
└────┬────┘     └────┬────┘
     │               │
     ▼               ▼
┌─────────┐     ┌─────────┐
│   SVD   │     │   SVD   │
│  (TDA)  │     │  (TDA)  │
└────┬────┘     └────┬────┘
     │               │
     └───────┬───────┘
             │
             ▼
    ┌─────────────────┐
    │ Singular Value  │
    │    Merging      │
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │  Dimensionality │
    │    Reduction    │
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │  Henon Chaotic  │◄── Secret Key
    │ Pixel Selection │    (x₀, y₀)
    └────────┬────────┘
             │
             ▼
    ┌─────────────────┐
    │   Inverse SVD   │
    │   Inverse DWT   │
    └────────┬────────┘
             │
             ▼
    Watermarked Frame ✅
```


<div align="center">
  <img src="https://github.com/user-attachments/assets/1f62dd73-7243-462e-a12e-af295214c1a5" alt="Algorithm Flow Diagram — DWT → SVD → Henon Pixel Shuffling → Watermarked Frame" width="80%">
  <p><em>Full embedding pipeline — DWT → SVD → Singular Value Merging → Henon chaotic pixel ordering → Inverse TDA → Watermarked Frame</em></p>
</div>

### The Henon Map — Why Chaos?

Instead of embedding the watermark sequentially (pixel 0,0 → 0,1 → 0,2...), the **Henon map** generates a chaotic permutation:

```
0,0 | 0,1 | 0,2 | 0,3
1,0 | 1,1 | 1,2 | 1,3     X = [2, 2, 2, 0, 2, 0, 3, 1, 0, 1, 3, 1, 1, 0, 3, 3]
2,0 | 2,1 | 2,2 | 2,3  →  Y = [0, 2, 1, 2, 3, 0, 0, 0, 3, 1, 1, 3, 2, 1, 2, 3]
3,0 | 3,1 | 3,2 | 3,3
```

The Henon map formula:
```
x(n+1) = 1 - a·x(n)² + y(n)
y(n+1) = b·x(n)
```

With parameters `a = 1.4`, `b = 0.3`. Without knowing the initial conditions **(x₀, y₀)**, it is mathematically impossible to reverse or predict the embedding pattern.

<div align="center">
  <img src="https://github.com/user-attachments/assets/27f62a5f-3b7b-422d-b34b-4f335df432a3" alt="Henon Map Attractor Visualization" width="60%">
  <p><em>Henon map attractor plot — chaotic distribution pattern used for pixel selection across 10,000+ iterations</em></p>
</div>



---

## 🎛️ Watermarking Modes

The operating mode also serves as part of the **security key** — an attacker who doesn't know the mode cannot extract the watermark.

| Mode | Description | Example |
|------|-------------|---------|
| **Mode 1** | Every nth frame | n=30 → frames 0, 30, 60, 90... |
| **Mode 2** | Frame range n to m | n=237, m=2018 |
| **Mode 3** | Specific frame indices | [23, 7, 2018, 1028] |
| **Mode 4** | All frames | First → Last |

---

## 🛡️ Security Architecture

The system provides **two independent layers of protection**:

**Layer 1 — Steganographic Obscurity**
The watermark is embedded in the frequency domain (DWT LL subband) at the singular value level. The visual change per pixel is imperceptible to the human eye.

**Layer 2 — Cryptographic Security**
Even if an attacker knows the embedding algorithm, they cannot recover the watermark without:
- The **Henon initial conditions** (x₀, y₀) — the chaotic key
- The **watermarking mode** — which frames were watermarked

---

## 📐 Mathematical Foundation

### SVD (Singular Value Decomposition)
```
A = U Σ Uᵀ
```
where `U` is orthogonal and `Σ` contains singular values representing critical image structure.

### DWT (Discrete Wavelet Transform)
```
f(t) = Σ c(j,k) · ψ(j,k)(t)
```
Separates the frame into LL (low freq), LH, HL, HH subbands. Embedding in LL maximizes robustness.

### Evaluation Metrics

**NC (Normalized Correlation):**
```
NC = [Σ W(i,j)·W'(i,j)] / [√Σ W(i,j)² · √Σ W'(i,j)²]
```

**PSNR:**
```
PSNR = 10 · log₁₀(MAX² / MSE)
```

**SSIM:**
```
SSIM(x,y) = [(2μₓμᵧ + C₁)(2σₓᵧ + C₂)] / [(μₓ² + μᵧ² + C₁)(σₓ² + σᵧ² + C₂)]
```

---

## ⚙️ Installation & Usage

### Requirements

```bash
pip install numpy opencv-python pywavelets matplotlib scipy
```

### Running the Watermarking

```python
# Embedding
python watermark_embed.py \
  --video input_video.mp4 \
  --watermark logo.png \
  --mode 1 \
  --n 30 \
  --key_x 0.1 \
  --key_y 0.1 \
  --output watermarked_video.mp4

# Extraction
python watermark_extract.py \
  --video watermarked_video.mp4 \
  --mode 1 \
  --n 30 \
  --key_x 0.1 \
  --key_y 0.1 \
  --output extracted_watermark.png
```

### Build from Source

```bash
git clone https://github.com/yasir237/chaotic-video-watermarking
cd chaotic-video-watermarking
pip install -r requirements.txt
python main.py
```

---

## 🧪 Attack Resistance Tests

The method was tested against the following attacks:

| Attack | NC Result | Status |
|--------|-----------|--------|
| Gaussian Noise | ~0.93 | ✅ Robust |
| Video Compression (H.264) | ~0.91 | ✅ Robust |
| Frame Cropping | ~0.90 | ✅ Robust |
| Gaussian Blur | ~0.92 | ✅ Robust |

---

## 🔮 Future Work

- [ ] Extend to H.265/HEVC compressed video streams
- [ ] Implement Lorenz system as alternative chaotic map for higher complexity scenarios
- [ ] Real-time watermarking for live video streams
- [ ] Web-based demo interface
- [ ] Multi-watermark support (multiple owners)
- [ ] GPU acceleration for high-resolution video processing

---

## 📚 References

- Horasan, F. (2022). A new image watermarking scheme using SVD decomposition. *Milli Eğitim Dergisi*
- Kumar, C. (2024). Optimized and secure digital image watermarking technique using Henon mapping in redundant domain. *Multimedia Tools and Applications*
- Alshoura, W. H., Zainol, Z., Teh, J. S., & Alawida, M. (2020). A new chaotic image watermarking scheme based on SVD and IWT. *IEEE Access*, 8, 192771–192783
- Asikuzzaman, M., & Pickering, M. R. (2018). An overview of digital video watermarking. *IEEE Transactions on Circuits and Systems for Video Technology*, 28(9), 2131–2143
- Evsutin, O., & Dzhanashia, K. (2022). Watermarking schemes for digital images: Robustness overview. *Signal Processing: Image Communication*
- Wu, D., Zhang, X., Wang, J., Li, L., & Feng, G. (2024). Novel robust video watermarking scheme based on concentric ring subband and visual cryptography with piecewise linear chaotic mapping. *IEEE Transactions on Circuits and Systems for Video Technology*, 34(10), 10281–10297
- Ponni, A. et al. (2017). Chaotic map based video watermarking using DWT and SVD. *ICICCT 2017*

---

## 📄 License

This project was developed as part of TÜBİTAK 2209-A research. All rights reserved.

---

<div align="center">

**Kırıkkale University — Computer Engineering Department**
**2024–2025 | TÜBİTAK 2209-A**

*Developed by Yasir Ayad Hamed AL RAWEE & Yusuf Ömer KARA*
*Under the supervision of Assoc. Prof. Dr. Fahrettin HORASAN*

</div>
