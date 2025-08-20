# Install ComfyUI on your Network Volume

1. [Create a RunPod Account](https://runpod.io).
2. Create a [RunPod Network Volume](https://www.runpod.io/console/user/storage).
3. Attach the Network Volume to a Secure Cloud [GPU pod](https://www.runpod.io/console/gpu-secure-cloud).
4. Select the RunPod Pytorch 2 template.
5. Deploy the GPU Cloud pod.
6. Once the pod is up, open a Terminal and install the required
   dependencies. This can either be done by using the installation
   script, or manually.

## Manual Installation

1. Install the ComfyUI:
```bash
# Clone the repo
cd /workspace
git clone --depth=1 https://github.com/comfyanonymous/ComfyUI.git comfyflux

# Upgrade Python
apt update
apt -y upgrade
apt-get install aria2 # for downloading models

# Ensure Python version is 3.10.12
python -V

# install uv & comfy-cli
pip install uv
uv pip install comfy-cli --system

# Create and activate venvflux
cd comfyflux
python -m venv /workspace/venvflux
source /workspace/venvflux/bin/activate

# Install Torch 
pip install --no-cache-dir torch==2.6.0+cu124 --index-url https://download.pytorch.org/whl/cu124 --no-deps
pip install --no-cache-dir torchvision==0.21.0+cu124 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu124

# Install ComfyUI
pip install -r requirements.txt

# Installing ComfyUI Manager
git clone https://github.com/ltdrdata/ComfyUI-Manager.git custom_nodes/ComfyUI-Manager
cd custom_nodes/ComfyUI-Manager
pip install -r requirements.txt

# Installing KJNodes
git clone https://github.com/kijai/ComfyUI-KJNodes.git custom_nodes/ComfyUI-KJNodes
cd custom_nodes/ComfyUI-KJNodes
pip install -r requirements.txt

# Installing rg-three nodes
git clone https://github.com/rgthree/rgthree-comfy.git custom_nodes/rgthree-comfy
cd custom_nodes/rgthree-comfy
pip install -r requirements.txt

```
2. Install the Serverless dependencies:
```bash
pip install requests runpod
pip install onnxruntime-gpu
pip install triton

```
3. Download models:
```bash
cd /workspace/comfyflux
mkdir -p models/vae models/unet models/clip models/text_encoders
# set your access token below
HUGGINGFACE_ACCESS_TOKEN=

wget -q --header="Authorization: Bearer ${HUGGINGFACE_ACCESS_TOKEN}" -O models/unet/flux1-dev.safetensors https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/flux1-dev.safetensors
aria2c -x16 -s16 -d /workspace/models/clip -o clip_l.safetensors --continue=true https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/clip_l.safetensors

aria2c -x16 -s16 -d /workspace/models/clip -o converted-flan-t5-xxl-Q5_K_M.gguf  --continue=true https://huggingface.co/dumb-dev/flan-t5-xxl-gguf/resolve/main/Q5/converted-flan-t5-xxl-Q5_K_M.gguf

wget -q --header="Authorization: Bearer ${HUGGINGFACE_ACCESS_TOKEN}" -O models/vae/ae.safetensors https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/ae.safetensors

# qwen image edit
aria2c -x16 -s16 -d /workspace/models/diffusion_models -o Qwen_Image_Edit-Q6_K.gguf --continue=true https://huggingface.co/QuantStack/Qwen-Image-Edit-GGUF/resolve/main/Qwen_Image_Edit-Q6_K.gguf

aria2c -x16 -s16 -d /workspace/models/text_encoders -o qwen_2.5_vl_7b_fp8_scaled.safetensors --continue=true https://huggingface.co/Comfy-Org/Qwen-Image_ComfyUI/resolve/main/split_files/text_encoders/qwen_2.5_vl_7b_fp8_scaled.safetensors

aria2c -x16 -s16 -d /workspace/models/vae -o qwen_image_vae.safetensors --continue=true https://huggingface.co/Comfy-Org/Qwen-Image_ComfyUI/resolve/main/split_files/vae/qwen_image_vae.safetensors

# qwen lightning lora
aria2c -x16 -s16 -d /workspace/models/loras -o Qwen-Image-Lightning-4steps-V1.0.safetensors --continue=true https://huggingface.co/lightx2v/Qwen-Image-Lightning/resolve/main/Qwen-Image-Lightning-4steps-V1.0.safetensors

# aria2c -x16 -s16 -d /workspace/models/text_encoders -o llama_3.1_8b_instruct_fp8_scaled.safetensors --continue=true https://huggingface.co/Comfy-Org/HiDream-I1_ComfyUI/resolve/main/split_files/text_encoders/llama_3.1_8b_instruct_fp8_scaled.safetensors

aria2c -x16 -s16 -d /workspace/models/text_encoders -o llama-3-8b-instruct-abliterated-dpomix.Q8_0.gguf --continue=true https://huggingface.co/mlabonne/Llama-3-8B-Instruct-abliterated-dpomix-GGUF/resolve/main/llama-3-8b-instruct-abliterated-dpomix.Q8_0.gguf

# LORAs
aria2c -x16 -s16 -d /workspace/models/loras -o flux-turbo-alpha.safetensors --continue=true https://huggingface.co/alimama-creative/FLUX.1-Turbo-Alpha/resolve/main/diffusion_pytorch_model.safetensors


# Download LORAs from civitai like so:
wget -O ./${filename}.safetensors "https://civitai.com/api/download/models/${modelID}$?type=Model&format=SafeTensor&token=${token}"

wget -O ./flux_anal_cowgirl.safetensors "https://civitai.com/api/download/models/1078264?type=Model&format=SafeTensor&token=${token}"

wget -O ./Flux_doggy_anal.safetensors "https://civitai.com/api/download/models/1047274?type=Model&format=SafeTensor&token=${token}"

wget -O ./FLUX_missio_pussy_v2.safetensors "https://civitai.com/api/download/models/1040415?type=Model&format=SafeTensor&token=${token}"

wget -O ./woman_dildo_flux_sevenof9_v1.safetensors "https://civitai.com/api/download/models/937813?type=Model&format=SafeTensor&token=${token}"

wget -O ./blow_flux_v2_Sevenof9.safetensors "https://civitai.com/api/download/models/740872?type=Model&format=SafeTensor&token=${token}"

wget -O /workspace/models/loras/comfyui_portrait_lora64.safetensors https://huggingface.co/ali-vilab/ACE_Plus/resolve/main/portrait/comfyui_portrait_lora64.safetensors

wget -O /workspace/models/loras/comfyui_subject_lora16.safetensors https://huggingface.co/ali-vilab/ACE_Plus/resolve/main/subject/comfyui_subject_lora16.safetensors

```
6. Create logs directory:
```bash
mkdir -p /workspace/logs-flux
```
