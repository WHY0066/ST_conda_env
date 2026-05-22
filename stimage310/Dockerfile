FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04
ENV DEBIAN_FRONTEND=noninteractive TZ=UTC PATH=/opt/conda/bin:$PATH

RUN apt-get update && apt-get install -y --no-install-recommends \
    wget bzip2 ca-certificates git curl build-essential \
    libgl1 libglib2.0-0 libsm6 libxext6 libxrender1 \
    libblas-dev liblapack-dev gfortran \
    && rm -rf /var/lib/apt/lists/*

RUN curl -L https://micro.mamba.pm/api/micromamba/linux-64/latest | \
    tar -xj -C /usr/local/bin --strip-components=1 bin/micromamba

COPY environment.yml /tmp/environment.yml
RUN micromamba env create -y -p /opt/conda -f /tmp/environment.yml && \
    micromamba clean -afy

# Allow direct git references in pyproject.toml (STimage needs this)
ENV HATCH_METADATA_ALLOW_DIRECT_REFERENCES=true

# Clone and install STimage from source
RUN git clone https://github.com/BiomedicalMachineLearning/STimage.git /opt/STimage && \
    cd /opt/STimage && \
    /opt/conda/bin/pip install -e .

WORKDIR /workspace
CMD ["/bin/bash"]
