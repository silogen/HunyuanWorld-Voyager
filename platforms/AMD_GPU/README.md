# HunyuanWorld-Voyager on AMD MI300X (ROCm7.0) – Quick Start

Follow the steps below to install and run HunyuanWorld-Voyager on AMD MI300X GPUs

## 1. Launch Container

```shell
docker run -it --rm \
  --device=/dev/kfd --device=/dev/dri --group-add video \
  -v $(pwd):/workspace -w /workspace \
  --shm-size 32g \
  amdsiloai/pytorch-xdit:v25.12
```

## 2. Dependencies and Installation

```shell
git clone https://github.com/silogen/HunyuanWorld-Voyager
cd HunyuanWorld-Voyager
# Use the AMD-adapted branch
git checkout feat/rocm-platform

# To create your own input conditions, you also need to install the following dependencies:
pip install --no-deps git+https://github.com/microsoft/MoGe.git
pip install scipy==1.11.4
pip install git+https://github.com/EasternJournalist/utils3d.git@a480806f58337da70d3c0df970b1df91ca152e61

# Dependencies
pip install pyexr==0.5.0 loguru==0.7.2 tensorboard==2.19.0 transformers==4.45

```

## 3. Download Weights

```shell
hf download tencent/HunyuanWorld-Voyager --local-dir ./ckpts
# Default model path is hardcoded to /root, modify the model path
export MODEL_BASE="./ckpts"
```

## 4. Inference

Setup complete. Now you can run inference using the standard script (see main README for examples).
