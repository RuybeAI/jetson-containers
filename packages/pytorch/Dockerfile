# Register QEMU for ARM emulation inside Docker Hub
FROM multiarch/qemu-user-static:register as qemu

# Stage 1: Provide QEMU binary to allow Docker Hub to emulate ARM64
FROM ubuntu:22.04 as base
COPY --from=qemu /usr/bin/qemu-aarch64-static /usr/bin/

# Stage 2: JetPack 6.2 base image (you pushed this to Docker Hub)
FROM ruybeai/l4t-jetpack:r36.4.0
COPY --from=base /usr/bin/qemu-aarch64-static /usr/bin/

# Avoid tzdata prompts
ENV DEBIAN_FRONTEND=noninteractive
ENV TORCH_CUDA_ARCH_LIST="8.7"
ENV MAX_JOBS=2

# Install build tools and dependencies
RUN apt-get update && apt-get install -y \
    python3-pip python3-dev \
    git cmake ninja-build \
    libopenblas-dev libomp-dev \
    && apt-get clean

# Clone and build PyTorch from source
RUN git clone --branch v2.3.0 https://github.com/pytorch/pytorch.git /pytorch && \
    cd /pytorch && \
    git submodule update --init --recursive && \
    pip3 install -r requirements.txt && \
    python3 setup.py install

# Default entrypoint
CMD ["python3"]
