# Stable Diffusion 1.5 Deployment with ComfyUI on Kaggle

## Overview

This project demonstrates the deployment and usage of Stable Diffusion 1.5 using ComfyUI on Kaggle's free Tesla T4 GPUs.

The primary objective was to understand the complete image generation workflow, from model deployment and GPU utilization to prompt-based image generation through a browser-accessible interface.

Instead of relying on paid AI image generation platforms, this setup allows Stable Diffusion to run on free cloud infrastructure provided by Kaggle.

---

## Problem Statement

Modern AI image generation models require significant computational resources, especially GPUs, which are often unavailable on entry-level personal computers.

The challenge is to run an open-source image generation model without investing in expensive hardware while still maintaining full control over the generation pipeline.

This project addresses that challenge by combining:

- Kaggle's free GPU resources
- Stable Diffusion 1.5
- ComfyUI
- Cloudflare Tunnel

to create a browser-accessible AI image generation environment.

---

## Why Stable Diffusion?

Stable Diffusion is one of the most popular open-source text-to-image models.

Benefits include:

- Free and open source
- Unlimited image generation
- Complete control over parameters
- Supports local and cloud deployment
- No subscription fees
- Large community and ecosystem

---

## Why Kaggle?

Running Stable Diffusion requires a dedicated GPU.

Instead of using local hardware, Kaggle provides free cloud resources including:

- Tesla T4 GPUs
- Cloud storage
- Notebook environments
- Internet access

This makes it possible to experiment with image generation models without owning high-end hardware.

---

## Project Architecture

```text
User Browser
      │
      ▼
Cloudflare Tunnel
      │
      ▼
Kaggle Notebook Environment
      │
      ▼
ComfyUI
      │
      ▼
Stable Diffusion 1.5 Model
      │
      ▼
Tesla T4 GPU
      │
      ▼
Generated Image
```

---

## Technologies Used

- Python
- Stable Diffusion 1.5
- ComfyUI
- Kaggle Notebooks
- Tesla T4 GPU
- Cloudflare Tunnel
- Hugging Face Model Hub

---

## Project Setup

### 1. Clone ComfyUI

```bash
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI
```

### 2. Install Required Dependencies

```bash
pip install -r requirements.txt
```

### 3. Download Stable Diffusion Model

```bash
mkdir -p models/checkpoints

wget -O models/checkpoints/v1-5-pruned-emaonly.safetensors \
https://huggingface.co/stable-diffusion-v1-5/stable-diffusion-v1-5
```

### 4. Launch ComfyUI

```python
import subprocess
import threading

def run():
    subprocess.run([
        "python",
        "main.py",
        "--listen",
        "0.0.0.0",
        "--port",
        "8188"
    ])

threading.Thread(target=run, daemon=True).start()
```

### 5. Create a Public Tunnel

```bash
./cloudflared-linux-amd64 tunnel --url http://localhost:8188 --no-autoupdate
```

The generated Cloudflare URL provides browser access to the ComfyUI interface.

---

## Understanding the Workflow

The image generation pipeline consists of the following nodes.

### 1. Load Checkpoint

Loads the Stable Diffusion model into memory.

Model Used:

```text
v1-5-pruned-emaonly.safetensors
```

---

### 2. CLIP Text Encode

Converts human-readable prompts into numerical representations that the model can understand.

Used for:

- Positive prompts
- Negative prompts

---

### 3. Empty Latent Image

Creates the initial latent space (random noise) that acts as the starting point for image generation.

---

### 4. KSampler

The core generation engine.

Responsibilities:

- Denoising
- Image synthesis
- Sampling operations
- Prompt-guided generation

---

### 5. VAE Decode

Converts latent representations into actual viewable images.

---

### 6. Save Image

Stores generated images to the output directory.

---

## Workflow Diagram

```text
Load Checkpoint
        │
        ▼
CLIP Text Encode
        │
        ▼
Empty Latent Image
        │
        ▼
KSampler
        │
        ▼
VAE Decode
        │
        ▼
Save Image
```

---

## Sample Prompt

### Positive Prompt

```text
A futuristic Hyderabad skyline at sunset, ultra realistic,
cinematic lighting, highly detailed, 8k
```

### Negative Prompt

```text
blurry, low quality, watermark, distorted
```

---

## Generated Output

Images are generated and stored in:

```text
ComfyUI/output
```

Example output:

```text
ComfyUI_00001_.png
```

---

## Screenshots

### Workflow

screenshots/workflow.png

### Generated Image

screenshots/generated_output.png

---

## Key Learnings

Through this project I gained practical exposure to:

- Stable Diffusion architecture
- GPU-based AI inference
- ComfyUI node-based workflows
- Prompt engineering
- Cloud-based deployment environments
- Cloudflare Tunnel integration
- AI image generation pipelines

---

## Future Improvements

Potential enhancements include:

- Stable Diffusion XL (SDXL)
- Flux Models
- ControlNet Integration
- LoRA Training
- Image-to-Image Generation
- AI Logo Generation Workflows
- Custom Model Fine-Tuning

---

## Disclaimer

This repository is intended for educational and learning purposes to understand modern AI image generation systems and deployment workflows.
