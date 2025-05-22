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
python -m venvflux /workspace/venvflux
source /workspace/venvflux/bin/activate

# Install Torch 
pip install --no-cache-dir torch==2.6.0+cu126 --index-url https://download.pytorch.org/whl/cu126 --no-deps
pip install --no-cache-dir torchvision==0.21.0+cu126 torchaudio==2.6.0 --index-url https://download.pytorch.org/whl/cu126

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
mkdir -p models/vae models/unet models/clip
# set your access token below
HUGGINGFACE_ACCESS_TOKEN=

wget -q --header="Authorization: Bearer ${HUGGINGFACE_ACCESS_TOKEN}" -O models/unet/flux1-dev.safetensors https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/flux1-dev.safetensors
aria2c -x16 -s16 -d /workspace/comfyflux/models/clip -o clip_l.safetensors --continue=true https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/clip_l.safetensors
aria2c -x16 -s16 -d /workspace/comfyflux/models/clip -o t5xxl_fp8_e4m3fn.safetensors --continue=true https://huggingface.co/comfyanonymous/flux_text_encoders/resolve/main/t5xxl_fp8_e4m3fn.safetensors
wget -q --header="Authorization: Bearer ${HUGGINGFACE_ACCESS_TOKEN}" -O models/vae/ae.safetensors https://huggingface.co/black-forest-labs/FLUX.1-dev/resolve/main/ae.safetensors

```
6. Create logs directory:
```bash
mkdir -p /workspace/logs-flux
```