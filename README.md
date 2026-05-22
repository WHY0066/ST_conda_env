# Containerized Conda Environments

This repository builds Apptainer/Docker container images for the following conda environments via GitHub Actions:

| Environment    | Python | Framework        | CUDA |
|----------------|--------|------------------|------|
| stimage310     | 3.10   | TensorFlow 2.15  | 11.8 |
| hovernet       | 3.9    | PyTorch 2.0.1    | 11.8 |
| loki_env       | 3.9    | PyTorch 2.3.1    | 12.1 |
| scist          | 3.9    | PyTorch 2.0.1    | 11.8 |
| path2space310  | 3.10   | PyTorch + many   | 12.1 |

## Usage (HPC with Apptainer)

```bash
module load apptainer
apptainer pull docker://ghcr.io/why0066/hovernet:latest
apptainer exec --nv -B $(pwd):/workspace hovernet_latest.sif bash
```

More details in `docs/` (coming soon).
