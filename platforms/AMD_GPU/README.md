# HunyuanWorld-Voyager on AMD MI300X (ROCm7.0) – Quick Start

Follow the steps below to install and run HunyuanWorld-Voyager on AMD MI300X GPUs

## 1. Launch Container

```shell
docker run -it --rm \
  --device=/dev/kfd --device=/dev/dri --group-add video \
  -v $(pwd):/workspace -w /workspace \
  --shm-size 32g \
  rocm/7.0:rocm7.0_pytorch_training_instinct_20250915
```

## 2. Dependencies and Installation

```shell
git clone https://github.com/Tencent-Hunyuan/HunyuanWorld-Voyager.git
cd HunyuanWorld-Voyager
python -m pip install -r requirements.txt
python -m pip install transformers==4.39.3

# To create your own input conditions, you also need to install the following dependencies:
pip install --no-deps git+https://github.com/microsoft/MoGe.git
pip install scipy==1.11.4
pip install git+https://github.com/EasternJournalist/utils3d.git@c5daf6f6c244d251f252102d09e9b7bcef791a38

# Additional requirements for Multi-GPU inference with torchrun
python -m pip install xfuser==0.4.2
## Fix the CUDA version parsing issue
sed -i 's/"CUDA_VERSION": lambda: version.parse(torch.version.cuda),/"CUDA_VERSION": lambda: version.parse(torch.version.cuda or "0.0.0"),/' /opt/venv/lib/python3.10/site-packages/xfuser/envs.py
## Update ring_flashinfer_attn.py, it has been fixed in recent commits but not released
pip uninstall yunchang -y
pip install git+https://github.com/feifeibear/long-context-attention.git@7a52abd669efb35e550680a239e1745b620b2bae
```

## 3. Download Weights

```shell
pip install "huggingface_hub[cli]"
huggingface-cli download tencent/HunyuanWorld-Voyager --local-dir ./ckpts
# Default model path is hardcoded to /root, modify the model path
export MODEL_BASE="./ckpts"
```

## 4. Inference

Setup complete. Now you can run inference using the standard script (see main README for examples).
