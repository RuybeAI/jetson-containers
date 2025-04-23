FROM nvcr.io/nvidia/l4t-jetpack:r36.4.0
COPY qemu-aarch64-static /usr/bin/

ENV DEBIAN_FRONTEND=noninteractive
ENV TORCH_CUDA_ARCH_LIST="8.7"
ENV MAX_JOBS=2

RUN apt-get update && apt-get install -y \
    python3-pip python3-dev \
    git cmake ninja-build \
    libopenblas-dev libomp-dev \
    && apt-get clean

RUN git clone --branch v2.3.0 https://github.com/pytorch/pytorch.git /pytorch && \
    cd /pytorch && \
    git submodule update --init --recursive && \
    pip3 install -r requirements.txt && \
    python3 setup.py install

CMD ["python3"]