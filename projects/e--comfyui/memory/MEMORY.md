# ComfyUI Project Memory

## Key Learnings

### IP-Adapter Weight for CelestReal
- IP-Adapter Plus (SDXL) with CelestReal v2.0: weight 0.8 or 0.6 causes the reference image's color/lighting to dominate the output (dark images)
- **Recommended weight: 0.2~0.4** for character consistency without overwhelming scene descriptions
- 0.2 = light reference, good scene freedom; 0.3-0.4 = balanced; 0.5+ = too strong

### ComfyUI API Format Loading
- When ComfyUI Desktop loads API format JSON workflows, node connections may not always render correctly in the UI
- The UI format (with pos, size, links arrays) is the canonical format after ComfyUI processes the workflow
- Always verify connections visually after loading an API format workflow

### NoobAI-XL-Vpred-v1.0 (CRITICAL)
- V-prediction model - **NOT compatible with Karras scheduler!**
- **Must use**: Euler sampler + Normal/SGM Uniform scheduler
- **CFG**: 4-6 (recommended 5.0), NOT 7+
- **Requires oversaturation correction**: RescaleCFG (multiplier=0.7) OR PAG (not both!)
- ComfyUI 1.28 auto-detects v-pred - do NOT add ModelSamplingDiscrete (double correction → dark images)
- CivitAI: https://civitai.com/models/833294/noobai-xl-nai-xl

### CelestReal v2.0 Model
- File: celestrealAnimeSemi_v20.safetensors (6.5GB)
- Already 2.5D semi-realistic by default - do NOT add "3d, realistic" tags (pushes too photorealistic)
- Uses Danbooru tags (English only)
- Quality prefix required: "masterpiece, best quality, newest, highres, absurdres,"
- CFG: 5.0, sampler: dpmpp_2m_sde, scheduler: karras

### Wan 2.2 I2V
- Supports Chinese prompts (unlike T2I models)
- fp8 version needed for 32GB VRAM
- LightX2V 4-step LoRA: reduces sampling from 20+ to 4 steps
- ModelSamplingSD3 shift=8 for noise schedule
- Output: 480x832 vertical for Douyin

### CivitAI Downloads
- Many models require authentication - direct download via CLI fails
- User must download manually from browser if no API key

### Web UI Pipeline
- Full 3-tab pipeline: anime-batch-generator.html (~1370 lines)
- URL: http://localhost:8000/anime-batch-generator.html
- Tab1: 角色设计 (T2I/I2I), Tab2: 分镜生成 (IP-Adapter 5场景), Tab3: 关键帧转视频 (I2V 5场景)
- Inter-tab transfer: Tab1→Tab2 character ref, Tab2→Tab3 keyframes
- Model dropdowns from /object_info/CheckpointLoaderSimple API
- Desktop_app dir: C:\Users\Administrator\AppData\Local\Programs\ComfyUI\resources\ComfyUI\web_custom_versions\desktop_app\
- Tab1/2 results: outputs["7"].images[0]; Tab3 videos: outputs["41"].gifs[0]

## Project Structure
- ComfyUI install: e:\comfyui (v1.28.8 Desktop, port 8000)
- GPU: RTX 5090 32GB
- Goal: Anime AI drama series for Douyin (TikTok China)
- Workflow files: e:\comfyui\user\default\workflows\
