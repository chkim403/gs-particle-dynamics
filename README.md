# Learning a Particle Dynamics Model with Real-world Videos

This repository will host the official code and dataset for our CVPR 2026 Findings paper:

**Learning a Particle Dynamics Model with Real-world Videos**  
Chanho Kim, Suhas V. Sumukh, and Li Fuxin  
CVPR 2026 Findings

Project page: [https://chkim403.github.io/gs_physics/](https://chkim403.github.io/gs_physics/)

## Release Plan

We plan to release the following items before CVPR 2026. Current release status:

- [x] Code
- [x] Instructions
- [x] Processed Dataset
- [ ] Raw Dataset
- [ ] Visualization Code

Note: the instructions are available and will be further updated as other release items become available.

## Dependencies

Later versions may also work, but these were the versions we used for our experiments.

- Python 3.9
- CUDA 12.4
- PyTorch 2.5.0
- PyTorch3D 0.7.8
- gsplat 1.4.0

The complete environment is provided in `environment.yml`. The smaller
`requirements.txt` lists the additional Python packages that are required when
you install the core CUDA/PyTorch stack manually.

## Installation

Follow these steps to set up the environment and install the required packages:

1. Create a conda environment and install PyTorch with CUDA 12.4 support.
   ```bash
   conda create -n your_env_name python=3.9
   conda activate your_env_name
   conda install pytorch==2.5.0 torchvision==0.20.0 torchaudio==2.5.0 pytorch-cuda=12.4 -c pytorch -c nvidia
   ```

2. Install PyTorch3D.
   ```bash
   pip install iopath scikit-image matplotlib imageio plotly opencv-python fvcore
   pip install "git+https://github.com/facebookresearch/pytorch3d.git@V0.7.8"
   ```

3. Install gsplat.
   ```bash
   pip install git+https://github.com/nerfstudio-project/gsplat.git@v1.4.0
   ```

4. Clone this repository:
   ```bash
   git clone https://github.com/chkim403/gs-particle-dynamics.git
   cd gs-particle-dynamics
   ```

5. Install the remaining project dependencies.
   ```bash
   pip install -r requirements.txt
   ```

Then set `PYTHONPATH` from the repository root before running the code:

```bash
export PYTHONPATH=$(pwd)
```

The experiment scripts also activate a conda environment internally. If you use
`scripts/run_experiment.sh`, update the `env_name` variable at the top of the
script to match your environment name. You may also need to adjust the conda
initialization or CUDA module-loading commands based on your system.

## Download Processed Dataset

The released code requires only the processed dataset.

We will host both the processed dataset and the raw dataset on Hugging Face. Until
the Hugging Face release is available, the processed dataset can be downloaded
temporarily from
[Dropbox](https://www.dropbox.com/scl/fi/l2rgo1twei9co2muapft7/processed_data.zip?rlkey=9zqu5onr2sucx79igxmidv2pf&dl=0).

See [docs/processed_dataset.md](docs/processed_dataset.md) for the processed dataset
structure and file descriptions. After downloading, update `data_dir` in
`configs/config.yaml` to point to the processed dataset directory.

## Raw Dataset

For those interested in accessing the original dataset files, we will also release
the raw dataset on Hugging Face. Details will be provided in
[docs/raw_dataset.md](docs/raw_dataset.md).

## Dataset License and Terms of Use

Before downloading or using the datasets, please review the
[dataset license](docs/dataset_license.md) and
[dataset terms of use](docs/dataset_terms_of_use.md).

## How to Run

Before running the code, update `configs/config.yaml` so that `data_dir` points to
the processed dataset directory. Set `exp_dir` to the directory where checkpoints
will be saved, and choose an `exp_name`, which is used as the checkpoint subfolder
name under `exp_dir`. Set `output_dir` to the directory where generated rollouts
from `tools/test.py` will be written.

### Train

Use `tools/train.py` with a config file and scenario name (`bowling` or `cube_stacks`):

```bash
python tools/train.py \
    --config_path configs/config.yaml \
    --scenario bowling
```

To fix a seed, pass the optional `--seed` flag:

```bash
python tools/train.py \
    --config_path configs/config.yaml \
    --scenario bowling \
    --seed 12345
```

Training saves the final checkpoint to:

```text
<exp_dir>/<exp_name>[_seed_<seed>]_<scenario>/epoch_<epoch>/model.pt
```

where `exp_dir`, `exp_name`, and the total number of training epochs are set in
`configs/config.yaml`.

### Test

Use `tools/test.py` with the same config, scenario, and seed used for training. The `--epoch`
argument selects which checkpoint epoch to load:

```bash
python tools/test.py \
    --config_path configs/config.yaml \
    --scenario bowling \
    --epoch 50
```

For a seeded run:

```bash
python tools/test.py \
    --config_path configs/config.yaml \
    --scenario bowling \
    --epoch 50 \
    --seed 12345
```

Testing writes rollout outputs under:

```text
<output_dir>/<scenario>/<exp_name>[_seed_<seed>]_epoch<epoch>_<scenario>/
```

### Using a Bash Script Instead

You can also edit the settings at the top of `scripts/run_experiment.sh` and run:

```bash
bash scripts/run_experiment.sh
```

The script activates the configured conda environment, sets `PYTHONPATH`, trains the model,
and then runs `tools/test.py` to generate rollout results from the selected checkpoint epoch
for each configured seed.

## License
This code is released under the MIT License.

## Acknowledgments

The `externals` folder includes code from the following repositories:

- [Dynamic3DGaussian](https://github.com/JonathonLuiten/Dynamic3DGaussians)
- [Physion](https://github.com/htung0101/Physion-particles)
- [PointConvFormer](https://github.com/apple/ml-pointconvformer)
- [RotationContinuity](https://github.com/papagina/RotationContinuity)

## Citation

If you find this project useful, please consider citing our paper:

```bibtex
@inproceedings{kim2026learning,
  title     = {Learning a Particle Dynamics Model with Real-world Videos},
  author    = {Kim, Chanho and Sumukh, Suhas V. and Fuxin, Li},
  booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR) Findings},
  year      = {2026}
}
