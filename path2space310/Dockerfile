FROM nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
ENV DEBIAN_FRONTEND=noninteractive TZ=UTC PATH=/opt/conda/bin:$PATH

RUN apt-get update && apt-get install -y --no-install-recommends \
    wget bzip2 ca-certificates git curl build-essential \
    libgl1 libglib2.0-0 libsm6 libxext6 libxrender1 \
    && rm -rf /var/lib/apt/lists/*

RUN curl -L https://micro.mamba.pm/api/micromamba/linux-64/latest | \
    tar -xj -C /usr/local/bin --strip-components=1 bin/micromamba

COPY environment.yml /tmp/environment.yml
RUN micromamba env create -y -p /opt/conda -f /tmp/environment.yml && \
    micromamba clean -afy

# Install PyTorch 2.6 + CUDA 12.4 from official PyTorch index
RUN /opt/conda/bin/pip install \
    --index-url https://download.pytorch.org/whl/cu124 \
    torch==2.6.0 torchvision==0.21.0

WORKDIR /workspace
CMD ["/bin/bash"]
