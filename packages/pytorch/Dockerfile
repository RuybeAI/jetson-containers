#---
# name: pytorch
# alias: torch
# group: pytorch
# config: config.py
# depends: [cuda, cudnn, numpy, onnx]
# test: [test.sh, test.py]
# docs: |
#  Containers for PyTorch with CUDA support.
#  Note that the [`l4t-pytorch`](/packages/l4t/l4t-pytorch) containers also include PyTorch, `torchvision`, and `torchaudio`.
#---
ARG BASE_IMAGE
FROM ${BASE_IMAGE}

# ───────────────────────────────────────────────────────────────────────────────
# Install prerequisites so that install.sh / build.sh can run
# ───────────────────────────────────────────────────────────────────────────────
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
      python3-pip \
      git && \
    rm -rf /var/lib/apt/lists/*

# ───────────────────────────────────────────────────────────────────────────────
# Build arguments
# ───────────────────────────────────────────────────────────────────────────────
ARG TORCH_CUDA_ARCH_LIST \
    TORCH_VERSION \
    PYTORCH_BUILD_VERSION \
    PIP_EXTRA_INDEX_URL \
    USE_NCCL=1 \
    USE_GLOO=1 \
    USE_MPI=0 \
    USE_FBGEMM=0 \
    USE_NNPACK=1 \
    USE_XNNPACK=1 \
    USE_PYTORCH_QNNPACK=1 \
    FORCE_BUILD=off

# ───────────────────────────────────────────────────────────────────────────────
# Environment variables
# ───────────────────────────────────────────────────────────────────────────────
ENV TORCH_CUDA_ARCH_LIST=${TORCH_CUDA_ARCH_LIST} \
    PIP_EXTRA_INDEX_URL=${PIP_EXTRA_INDEX_URL} \
    TORCH_HOME=/data/models/torch

# ───────────────────────────────────────────────────────────────────────────────
# Copy the upstream helper scripts and invoke them
# ───────────────────────────────────────────────────────────────────────────────
COPY install.sh build.sh /tmp/pytorch/
RUN /tmp/pytorch/install.sh || /tmp/pytorch/build.sh