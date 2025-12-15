# Wan 2.1 Video Generation Workflows

Professional ComfyUI workflows for AI video generation using Wan 2.1 models.

## Available Workflows

| Workflow | Type | Use Case | Speed |
|----------|------|----------|-------|
| **wan21_t2v_fp8_superquality.json** | Text-to-Video | Generate video from text description only | ~15-25 min |
| **text_to_video_optimized.json** | Text-to-Video | Fast T2V with LoRA acceleration | ~4-5 min |
| **professional_t2v_pipeline.json** | Text-to-Video | Advanced T2V with quality presets | ~3-30 min |
| **image_to_video_optimized.json** | Image-to-Video | Animate any image | ~4-5 min |
| **image_to_video_simple.json** | Image-to-Video | Simple I2V for beginners | ~4-5 min |

---

## NEW: Super Quality Text-to-Video (FP8)

**File:** `wan21_t2v_fp8_superquality.json`

Generate videos purely from text with maximum quality settings:

| Setting | Value | Purpose |
|---------|-------|---------|
| **Model** | `wan2.1_t2v_14B_fp8_e4m3fn` | FP8 quantized (50% less VRAM) |
| **Text Encoder** | `umt5_xxl_fp8_e4m3fn_scaled` | Memory efficient |
| **Resolution** | 832×480 (16:9) | YouTube standard |
| **Frames** | 81 (~5 sec) | Good duration |
| **Steps** | 30 | Maximum quality |
| **CFG** | 6.0 | High guidance |
| **Scheduler** | beta | Better temporal consistency |
| **CRF** | 17 | High quality video |

### Required Models (FP8 Version)

```bash
# Download FP8 diffusion model (~14GB)
wget -P models/diffusion_models/ "https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.1_t2v_14B_fp8_e4m3fn.safetensors"

# Download FP8 text encoder (~5GB)
wget -P models/text_encoders/ "https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/text_encoders/umt5_xxl_fp8_e4m3fn_scaled.safetensors"

# Download VAE (~300MB)
wget -P models/vae/ "https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/vae/wan_2.1_vae.safetensors"
```

---

## Image-to-Video Workflows

Transform any image into a 5-10 second AI-generated video using Wan 2.1 VACE model in ComfyUI.

## What You Get

- **Input**: Any image (PNG, JPG, WebP)
- **Output**: ~6 second video (MP4) with smooth motion
- **Quality**: 512x512 resolution at 16fps

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

## Quick Setup (15 minutes)

### Step 1: Install ComfyUI

```bash
# Clone ComfyUI
git clone https://github.com/comfyanonymous/ComfyUI.git
cd ComfyUI

# Create virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
# OR: venv\Scripts\activate  # Windows

# Install requirements
pip install -r requirements.txt
```

### Step 2: Download Models

Download these 4 files and place them in the correct folders:

#### Diffusion Model (~28GB)
```
ComfyUI/models/diffusion_models/
```
- [wan2.1_vace_14B_fp16.safetensors](https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/diffusion_models/wan2.1_vace_14B_fp16.safetensors)

#### Text Encoder (~10GB)
```
ComfyUI/models/text_encoders/
```
- [umt5_xxl_fp16.safetensors](https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/text_encoders/umt5_xxl_fp16.safetensors)

#### VAE (~300MB)
```
ComfyUI/models/vae/
```
- [wan_2.1_vae.safetensors](https://huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged/resolve/main/split_files/vae/wan_2.1_vae.safetensors)

#### LoRA (Speed Boost) (~500MB)
```
ComfyUI/models/loras/
```
- [Wan21_CausVid_14B_T2V_lora_rank32.safetensors](https://huggingface.co/Kijai/WanVideo_comfy/resolve/main/Wan21_CausVid_14B_T2V_lora_rank32.safetensors)

### Step 3: Copy Workflow

Copy `image_to_video_simple.json` to your ComfyUI folder (or anywhere accessible).

---

## How to Use

### Start ComfyUI
```bash
cd ComfyUI
python main.py
```
Open browser: `http://127.0.0.1:8188`

### Generate Your Video

1. **Load Workflow**: Drag `image_to_video_simple.json` onto ComfyUI window

2. **Upload Image**: Click the green "UPLOAD YOUR IMAGE HERE" node and select your image
   - Best results: Images with a clear subject on solid/simple background
   - Supported: PNG, JPG, WebP

3. **Edit Prompt** (optional): Describe the motion you want:
   ```
   Examples:
   - "The person walks forward slowly, hair blowing in the wind"
   - "The car drives down the road, wheels spinning"
   - "The flower blooms and sways gently in the breeze"
   - "The character turns their head and smiles"
   ```

4. **Click "Queue"**: Press the Queue button and wait (~4-5 minutes on RTX 4090)

5. **Find Your Video**: Output saved to `ComfyUI/output/video/`

---

## File Structure

```
ComfyUI/
├── models/
│   ├── diffusion_models/
│   │   └── wan2.1_vace_14B_fp16.safetensors
│   ├── text_encoders/
│   │   └── umt5_xxl_fp16.safetensors
│   ├── loras/
│   │   └── Wan21_CausVid_14B_T2V_lora_rank32.safetensors
│   └── vae/
│       └── wan_2.1_vae.safetensors
├── output/
│   └── video/        <-- Your videos save here
└── main.py
```

---

## Customization

### Change Video Length

In the `WanVaceToVideo` node, edit the `length` value:
- **81 frames** = ~5 seconds
- **97 frames** = ~6 seconds (default)
- **129 frames** = ~8 seconds
- **161 frames** = ~10 seconds

### Change Resolution

In the `WanVaceToVideo` node:
- **512x512** - Fast, good quality (default)
- **768x768** - Higher quality, slower
- **480p** (832x480) - Widescreen

### Adjust LoRA Strength

In the `LoraLoader` node, the `strength_model` value (0.3-0.7):
- **0.3** - More stable, closer to original
- **0.5** - Balanced
- **0.7** - More motion, may be less stable

---

## Troubleshooting

### Out of Memory (OOM)
- Lower resolution to 512x512
- Reduce frame count to 81
- Close other GPU applications
- Use fp8 text encoder instead of fp16

### Video is Shaky/Blurry
- Lower LoRA strength to 0.3
- Use simpler motion prompts
- Ensure input image has clear subject

### Models Not Loading
- Verify model files are in correct folders
- Check file names match exactly
- Restart ComfyUI

### Generation Takes Too Long
- The LoRA reduces time from 40min to ~4min
- First run is slower (model loading)
- 512x512 is faster than 768x768

---

## Tips for Best Results

1. **Image Selection**
   - Clear subject with simple background works best
   - Avoid complex scenes with many elements
   - Higher quality input = better output

2. **Prompt Writing**
   - Describe motion, not appearance
   - Be specific: "walks forward" not "moves"
   - Include camera movement if desired

3. **Iteration**
   - Try different seeds (randomize is enabled)
   - Adjust LoRA strength for different effects
   - Experiment with prompt variations

---

## Credits

- **Model**: [Wan-AI](https://huggingface.co/Wan-AI) - Wan 2.1 VACE
- **LoRA**: [Kijai](https://huggingface.co/Kijai) - CausVid acceleration
- **Framework**: [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
