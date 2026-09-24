# Stable Diffusion with ComfyUI on Kaggle

## Overview

This repository documents the setup and deployment of Stable Diffusion 1.5 using ComfyUI on Kaggle's free Tesla T4 GPUs.

The project demonstrates how to:

- Run Stable Diffusion without a local GPU
- Deploy ComfyUI on Kaggle
- Download and use the Stable Diffusion 1.5 model
- Expose the interface securely using Cloudflare Tunnel
- Generate images from text prompts

---

## Architecture

Browser
↓
Cloudflare Tunnel
↓
Kaggle Notebook
↓
ComfyUI
↓
Stable Diffusion 1.5
↓
Tesla T4 GPU
↓
Generated Image

---

## Technologies Used

- Python
- Kaggle Notebooks
- ComfyUI
- Stable Diffusion 1.5
- Cloudflare Tunnel

---

## Setup Steps

### 1. Clone ComfyUI

```bash
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Download Stable Diffusion Model

```bash
mkdir -p models/checkpoints
wget -O models/checkpoints/v1-5-pruned-emaonly.safetensors <MODEL_URL>
```

### 4. Start ComfyUI

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

### 5. Start Cloudflare Tunnel

```bash
./cloudflared-linux-amd64 tunnel --url http://localhost:8188 --no-autoupdate
```

---

## Workflow

Load Checkpoint
→ CLIP Text Encode
→ Empty Latent Image
→ KSampler
→ VAE Decode
→ Save Image

---

## Sample Prompt

Positive Prompt:

A futuristic Hyderabad skyline at sunset, ultra realistic, cinematic lighting, highly detailed

Negative Prompt:

blurry, low quality, watermark, distorted

---

## Output

Images are generated and stored inside:

```text
ComfyUI/output
```

---

## Learning Outcomes

- Understanding Stable Diffusion pipelines
- Using ComfyUI node-based workflows
- Working with Kaggle GPU environments
- Exposing local services using Cloudflare Tunnel
- Generating AI images from text prompts
