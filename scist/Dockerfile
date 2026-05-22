FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive \
    TZ=UTC \
    PATH=/opt/conda/bin:$PATH \
    CONDA_AUTO_UPDATE_CONDA=false

RUN apt-get update && apt-get install -y --no-install-recommends \
    wget bzip2 ca-certificates git curl build-essential \
    libgl1 libglib2.0-0 libsm6 libxext6 libxrender1 \
    && rm -rf /var/lib/apt/lists/*

RUN curl -L https://micro.mamba.pm/api/micromamba/linux-64/latest | \
    tar -xj -C /usr/local/bin --strip-components=1 bin/micromamba

COPY environment.yml /tmp/environment.yml
RUN micromamba env create -y -p /opt/conda -f /tmp/environment.yml && \
    micromamba clean -afy

RUN echo "source /opt/conda/etc/profile.d/conda.sh && conda activate /opt/conda" >> /etc/profile.d/conda.sh

ENV PATH=/opt/conda/bin:$PATH

WORKDIR /workspace
CMD ["/bin/bash"]
