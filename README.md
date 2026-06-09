# Astronomical Image Denoising Using Physics-Guided Multi U-Net Models

## Overview

Astronomical images captured by telescopes are often degraded by multiple sources of sensor noise, making it difficult to observe faint celestial structures and fine spatial details. This project presents a **physics-guided deep learning framework** for astronomical image denoising that combines realistic CCD noise simulation with a **multi-expert U-Net architecture** and a **learnable fusion network**.

The framework trains multiple specialized U-Net models, each dedicated to removing a specific type of noise commonly found in astronomical imaging:

- Photon Shot Noise
- Read-Out Noise
- Dark Current Noise
- Composite (Mixed) Noise

The outputs of these expert models are fused using a lightweight **1×1 Convolution Fusion Layer**, producing a final denoised image that preserves faint astrophysical structures while improving overall image quality.

---

## Research Paper

This repository accompanies our research paper:

**Astronomical Image Denoising Using Physics-Guided Multi U-Net Models**

### Abstract

Astronomical images captured by CCD sensors are inherently affected by photon shot noise, read-out noise, and dark-current noise. These degradations often obscure faint celestial structures and reduce scientific usability. This work introduces a physics-guided denoising framework that combines realistic CCD noise simulation with multiple specialized U-Net denoisers. Individual models are trained for specific noise components, and their outputs are integrated using a learnable 1×1 convolutional fusion layer. Experiments on Hubble Space Telescope imagery demonstrate superior reconstruction quality, achieving **32.90 dB PSNR** and **0.840 SSIM**, outperforming individual denoisers while preserving faint astrophysical details.

---

## Key Features

- Physics-guided astronomical noise simulation
- Multiple specialized U-Net denoisers
- Learnable fusion of expert predictions
- Dynamic noise generation during training
- Preservation of faint celestial structures
- Evaluation using PSNR and SSIM metrics
- Trained and tested on Hubble Space Telescope imagery

---

## Dataset

The project uses the **Top 100 Hubble Telescope Images Dataset** available on Kaggle.

### Dataset Contents

- Galaxies
- Nebulae
- Star Clusters
- Deep-Sky Objects

### Preprocessing

- Convert images to grayscale
- Resize while maintaining aspect ratio
- Maximum dimension: 512 × 512
- Random extraction of 128 × 128 patches
- Dynamic noise injection during training

### Dataset Source

https://www.kaggle.com/datasets/redwankarimsony/top-100-hubble-telescope-images

---

## Physics-Based Noise Modeling

To simulate realistic CCD imaging conditions, the framework models three primary noise sources.

### 1. Photon Shot Noise

Photon arrivals follow a Poisson distribution:

λ = I × t × α

Nshot ~ Poisson(λ)

where:

- I = image intensity
- t = exposure time
- α = photon conversion factor

### 2. Read-Out Noise

Electronic sensor noise modeled as:

Nron ~ N(0, σ²ron)

### 3. Dark Current Noise

Thermally generated electrons modeled as:

Ndc ~ N(0, σ²dc × t)

### Composite Noise

The final noisy image is generated as:

Inoisy = (Nshot + Nron + Ndc) / (t × α)

This produces physically realistic training samples that closely resemble telescope observations.

---

## Model Architecture

### Expert Networks

Four lightweight U-Net models are trained independently:

| Model | Purpose |
|---------|----------|
| UNet_SN | Shot Noise Removal |
| UNet_RON | Read-Out Noise Removal |
| UNet_DN | Dark Current Noise Removal |
| UNet_ASTRO | Composite Noise Removal |

Each U-Net consists of:

- Encoder Blocks
- Bottleneck Layer
- Decoder Blocks
- Skip Connections
- Final 1×1 Convolution Layer

---

## Fusion Network

After training the expert denoisers:

1. Predictions from all U-Nets are generated.
2. Outputs are concatenated channel-wise.
3. A learnable 1×1 convolution layer combines them.

```text
Ifused = W * [ISN, IRON, IDN, IASTRO]
```

This allows the model to leverage the strengths of each specialist denoiser.

---

## Training Details

### Loss Function

Mean Squared Error (MSE)

```text
L = ||Iclean − Ipred||²
```

### Optimizer

- Adam Optimizer
- Mixed Precision Training

### Training Strategy

- Dynamic noise generation every epoch
- Random patch extraction
- Supervised learning using clean/noisy image pairs

---

## Results

| Model | PSNR (dB) | SSIM |
|---------|-----------|--------|
| UNet_SN | 23.68 | 0.470 |
| UNet_RON | 23.01 | 0.400 |
| UNet_DN | 30.25 | 0.750 |
| UNet_ASTRO | 31.16 | 0.820 |
| Fusion Model | **32.90** | **0.840** |

### Key Findings

- Dark-current denoising performs best among individual experts.
- Composite-noise training improves robustness.
- Fusion network consistently outperforms all standalone models.
- Better preservation of faint galactic arms and nebular structures.

---

## Workflow

```text
Clean Hubble Images
          │
          ▼
Physics-Guided Noise Simulation
          │
          ▼
 ┌─────────────────────────┐
 │  UNet_SN (Shot Noise)   │
 ├─────────────────────────┤
 │ UNet_RON (Read-Out)     │
 ├─────────────────────────┤
 │ UNet_DN (Dark Current)  │
 ├─────────────────────────┤
 │ UNet_ASTRO (Composite)  │
 └─────────────────────────┘
          │
          ▼
Concatenation of Outputs
          │
          ▼
1×1 Convolution Fusion Layer
          │
          ▼
Final Denoised Image
```

---

## Repository Structure

```text
.
├── Notebook.ipynb
├── README.md
├── paper/
│   └── research_paper.pdf
├── models/
├── outputs/
├── figures/
└── requirements.txt
```

---

## Technologies Used

- Python
- PyTorch
- NumPy
- OpenCV
- Matplotlib
- Scikit-Image
- Jupyter Notebook

---

## Applications

- Astronomical Image Enhancement
- Telescope Data Processing Pipelines
- Deep-Sky Object Observation
- Space Telescope Imaging
- Scientific Image Restoration
- Astrophysical Data Analysis

---

## Future Work

- Transformer-based denoising architectures
- Diffusion-based restoration models
- Cosmic ray artifact removal
- Atmospheric turbulence correction
- Real-time telescope pipeline integration
- Uncertainty-aware scientific reconstruction

---

## Authors

**Anirudh Inamdar**  
KLE Technological University

**Savitru Gadagkar**  
KLE Technological University

**Prateek Ariga**  
KLE Technological University

**Shreyas Hurakadli**  
KLE Technological University

---

## Citation

If you use this work in your research, please cite:

```bibtex
@article{inamdar2026astronomical,
  title={Astronomical Image Denoising Using Physics-Guided Multi U-Net Models},
  author={Inamdar, Anirudh and Gadagkar, Savitru and Ariga, Prateek and Hurakadli, Shreyas},
  year={2026}
}
```

---

## License

This project is released under the MIT License.
