# AI Video Generation Toolkit

Transform images into videos, generate videos from text, or control motion with pose/depth videos using Wan 2.1/2.2 models in ComfyUI.

## Available Workflows

| Workflow | Input | Output | Best For |
|----------|-------|--------|----------|
| `image_to_video_simple.json` | Image | 5-10s video | Quick image animation |
| `image_to_video_optimized.json` | Image | MP4 + WebP | Production I2V with guides |
| `text_to_video_optimized.json` | Text only | MP4 + WebP | Generate from description |
| `professional_t2v_pipeline.json` | Text only | MP4 + WebP | Advanced T2V with presets |
| `wan22_fun_control.json` | Control video + Start image | MP4 + WebP | Pose/depth controlled generation |

---

## Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| GPU VRAM | 12GB | 24GB (RTX 4090) |
| RAM | 16GB | 32GB |
| Storage | 30GB free | 50GB free |
| OS | Windows/Linux/Mac | Windows/Linux |

**Software Required:**
- [ComfyUI](https://github.com/comfyanonymous/ComfyUI) (v0.3.34+)
- Python 3.10+

---

## Quick Setup

### Step 1: Install ComfyUI

```bash
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI
python -m venv venv
source venv/bin/activate  # Linux/Mac
# OR: venv\Scripts\activate  # Windows
pip install -r requirements.txt
```

### Step 2: Download Models

Choose models based on your workflow:

#### For Image-to-Video (I2V):
```bash
# Diffusion Model
wget -P models/diffusion_models/ https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.1_vace_14B_fp16.safetensors

# Text Encoder
wget -P models/text_encoders/ https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/text_encoders/umt5_xxl_fp16.safetensors

# VAE
wget -P models/vae/ https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/vae/wan_2.1_vae.safetensors

# Speed LoRA
wget -P models/loras/ https://huggingface.co/Kijai/WanVideo_comfy/resolve/main/Wan21_CausVid_14B_T2V_lora_rank32.safetensors
```

#### For Text-to-Video (T2V):
```bash
# T2V Diffusion Model (instead of VACE)
wget -P models/diffusion_models/ https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.1_t2v_14B_fp16.safetensors
```

#### For Wan 2.2 Fun Control:
```bash
# High Noise Model (recommended)
wget -P models/diffusion_models/ https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.2_fun_control_high_noise_14B_fp8_scaled.safetensors

# Low Noise Model (alternative)
wget -P models/diffusion_models/ https://huggingface.co/Comfy-Org/Wan_2.2_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.2_fun_control_low_noise_14B_fp8_scaled.safetensors

# FP8 Text Encoder (more efficient)
wget -P models/text_encoders/ https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/text_encoders/umt5_xxl_fp8_e4m3fn_scaled.safetensors

# CLIP Vision (for start frame)
wget -P models/clip_vision/ https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/clip_vision/clip_vision_h.safetensors

# Matching Speed LoRAs
wget -P models/loras/ https://huggingface.co/Kijai/WanVideo_comfy/resolve/main/wan2.2_i2v_lightx2v_4steps_lora_v1_high_noise.safetensors
wget -P models/loras/ https://huggingface.co/Kijai/WanVideo_comfy/resolve/main/wan2.2_i2v_lightx2v_4steps_lora_v1_low_noise.safetensors
```

---

## Workflow Guides

### Image-to-Video (Simple)

1. Load `image_to_video_simple.json` in ComfyUI
2. Upload your image
3. Edit the prompt to describe desired motion
4. Click Queue

**Best for:** Quick animations from existing images

### Text-to-Video (Optimized)

1. Load `text_to_video_optimized.json` in ComfyUI
2. Write detailed prompt describing scene and motion
3. Adjust resolution and frame count
4. Click Queue

**Best for:** Creating videos purely from text descriptions

### Wan 2.2 Fun Control

Control video generation with pose, depth, or edge videos!

1. Load `wan22_fun_control.json` in ComfyUI
2. Upload a **start frame** image (sets appearance)
3. Upload a **control video** (defines motion)
4. Write prompt describing the output
5. Click Queue

**Control Types:**
| Type | Use For |
|------|---------|
| Pose (OpenPose) | Character movement, dancing |
| Depth | 3D scene structure |
| Canny | Edge-based control, line art |
| MLSD | Architecture, straight lines |

**Important:** Match model and LoRA types!
- High Noise Model → High Noise LoRA
- Low Noise Model → Low Noise LoRA

---

## Quick Reference

### Resolution Options
| Size | Ratio | VRAM | Use |
|------|-------|------|-----|
| 512x320 | 16:9 | ~15GB | Testing |
| 832x480 | 16:9 | ~25GB | Standard |
| 480x832 | 9:16 | ~25GB | TikTok/Reels |
| 1280x720 | 16:9 | ~40GB | High quality |

### Frame Count → Duration @16fps
| Frames | Duration |
|--------|----------|
| 33 | ~2 sec |
| 49 | ~3 sec |
| 81 | ~5 sec |
| 97 | ~6 sec |
| 129 | ~8 sec |
| 161 | ~10 sec |

### LoRA Strength Guide
| Value | Effect |
|-------|--------|
| 0.3 | Stable, slower (~5min) |
| 0.4 | Balanced (~4min) |
| 0.5 | Fast, may wobble (~3min) |

---

## Troubleshooting

### Out of Memory (OOM)
- Lower resolution to 512x320
- Reduce frame count to 33-49
- Use fp8 models instead of fp16
- Close other GPU applications

### Blurry/Shaky Video
- Lower LoRA strength to 0.3
- Increase steps to 6-8
- Use simpler motion descriptions
- Ensure input image is high quality

### Models Not Loading
- Verify files are in correct folders
- Check file names match exactly
- Restart ComfyUI

### Fun Control Not Working
- Ensure model and LoRA types match (high→high, low→low)
- Control video frame count must match output frames
- Use high contrast control videos

---

## File Structure

```
ComfyUI/
├── models/
│   ├── diffusion_models/
│   │   ├── wan2.1_vace_14B_fp16.safetensors (I2V)
│   │   ├── wan2.1_t2v_14B_fp16.safetensors (T2V)
│   │   └── wan2.2_fun_control_*_14B_fp8_scaled.safetensors
│   ├── text_encoders/
│   │   └── umt5_xxl_*.safetensors
│   ├── clip_vision/
│   │   └── clip_vision_h.safetensors
│   ├── loras/
│   │   ├── Wan21_CausVid_14B_T2V_lora_rank32.safetensors
│   │   └── wan2.2_i2v_lightx2v_4steps_lora_v1_*.safetensors
│   └── vae/
│       └── wan_2.1_vae.safetensors
├── output/
│   ├── video/
│   ├── t2v_video/
│   └── fun_control/
└── main.py
```

---

## Credits

- **Models**: [Wan-AI](https://huggingface.co/Wan-AI) - Wan 2.1/2.2
- **LoRAs**: [Kijai](https://huggingface.co/Kijai) - Speed acceleration
- **Framework**: [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
- **Repackaged**: [Comfy-Org](https://huggingface.co/Comfy-Org)
