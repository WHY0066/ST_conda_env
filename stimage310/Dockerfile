FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04
ENV DEBIAN_FRONTEND=noninteractive TZ=UTC PATH=/opt/conda/bin:$PATH

# Remove compat libcuda.so (interferes with apptainer's --nv driver injection)
RUN rm -rf /usr/local/cuda-11.8/compat /usr/local/cuda/compat 2>/dev/null || true

# Let TF find cuDNN (in conda lib) and CUDA runtime
# Note: /.singularity.d/libs is where apptainer --nv injects host driver
ENV LD_LIBRARY_PATH=/.singularity.d/libs:/opt/conda/lib:/usr/local/cuda/lib64:/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH
ENV XLA_FLAGS=--xla_gpu_cuda_data_dir=/usr/local/cuda

RUN apt-get update && apt-get install -y --no-install-recommends \
    wget bzip2 ca-certificates git curl build-essential \
    libgl1 libglib2.0-0 libsm6 libxext6 libxrender1 \
    libblas-dev liblapack-dev gfortran \
    && rm -rf /var/lib/apt/lists/*

RUN curl -L https://micro.mamba.pm/api/micromamba/linux-64/latest | \
    tar -xj -C /usr/local/bin --strip-components=1 bin/micromamba

# Step 1: install conda packages from yaml
COPY environment.yml /tmp/environment.yml
RUN micromamba env create -y -p /opt/conda -f /tmp/environment.yml && \
    micromamba clean -afy

# Step 2: install pip packages separately
COPY pip_requirements_full.txt /tmp/pip_requirements_full.txt
RUN /opt/conda/bin/pip install --no-cache-dir -r /tmp/pip_requirements_full.txt

# Step 3: Clone STimage and install with --no-deps
RUN git clone https://github.com/BiomedicalMachineLearning/STimage.git /opt/STimage && \
    cd /opt/STimage && \
    printf '\n[tool.hatch.metadata]\nallow-direct-references = true\n' >> pyproject.toml && \
    /opt/conda/bin/pip install --no-deps -e .

WORKDIR /workspace
CMD ["/bin/bash"]
