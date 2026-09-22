# Jacobian-Based Measurement

**Jacobian-Based Measurement of Local Input-Equivalent Sensitivity of ECG Encoders Under Federated Update Perturbations**

Reproducibility source distribution for manuscript TIM-26-07850.
Repository: https://github.com/am-dimnet/Jacobian-Based-Measurement

## Included in this repository

- Controlled training, Jacobian estimation, Monte Carlo matching and uncertainty-analysis implementations.
- Training and measurement job matrices, environment specifications, preprocessing rules and record/split manifests.
- Small retained numerical summaries, table validation, plotting scripts and manuscript figure resources.
- Historical source snapshots, explicitly separated from supported controlled-study entry points.
- A SHA-256 inventory of the separately stored inputs, checkpoints and raw numerical arrays.

The repository does **not** contain the large waveform files, processed input arrays, checkpoints or raw measurement arrays. Complete numerical replay requires those external files. Their public archive URL has not yet been published or verified. A source-only checkout cannot reproduce all reported results by itself. See [EXTERNAL_ASSETS.md](EXTERNAL_ASSETS.md).

## Check the source distribution

Python 3.12 is recommended; this check uses only the standard library.

```bash
python scripts/verify_package.py
```

This verifies the repository manifest, English text and Python syntax. It does not train a model, execute inference or certify experimental results.

## Assemble a complete package from available evidence

External files must preserve the relative paths in `EXTERNAL_ASSETS.json`. For the author's local layout, run from this `GitHub` directory and use `..` as the asset root:

```bash
python scripts/assemble_package.py --assets-dir .. --check-only
python scripts/assemble_package.py --assets-dir .. --destination /path/to/new-complete-package
```

For a separately downloaded evidence archive, replace `..` with its extraction directory. The helper checks every external SHA-256, refuses an existing destination and assembles a separate package without running experiments. It does not move or alter the external evidence. On supported macOS filesystems it uses independent copy-on-write clones; elsewhere it copies files.

Then follow [docs/WORKFLOW.md](docs/WORKFLOW.md) from the assembled package. A typical retained-output workflow is:

```bash
cd /path/to/new-complete-package
conda env create -f environment.yml
conda activate tim2607850
python -m pip install -r requirements.txt
# Install the appropriate recorded PyTorch build when needed:
python -m pip install -r requirements-cuda.txt
python scripts/verify_package.py --require-assets
python prepare_reproduction.py --mode analysis --destination /path/to/new-analysis
python scripts/reproduce.py --workspace /path/to/new-analysis --stage validate --execute
python scripts/reproduce.py --workspace /path/to/new-analysis --stage analysis --execute
python scripts/reproduce.py --workspace /path/to/new-analysis --stage figures --execute
```

Training and measurement commands are documented in the workflow guide. CUDA is required by those evaluation entry points. The recorded server used Python 3.12.3, PyTorch 2.8.0+cu128 and Tesla V100S GPUs. No claim of bitwise portability or matching timing across hardware is made.

## Scope and limitations

The controlled study has 24 training runs, 48 initial measurement jobs, 15 additional evaluations and 3 matching-only bracket extensions. Historical saved-prediction analyses are separate: missing original checkpoints and source-cache mappings prevent complete historical retraining replay. A new training run cannot recover that missing history. Nominal dataset voltages do not establish an instrument-calibration chain.

See [REPRODUCIBILITY.md](REPRODUCIBILITY.md) for result-to-code mappings, estimator definitions and evidence boundaries. The `manuscript/` directory is a dated source snapshot; publishing it does not assert journal acceptance.

## Data and software terms

Source releases are PTB-XL v1.0.3 (DOI `10.13026/kfzx-aw45`) and ECG-arrhythmia v1.0.0 (DOI `10.13026/wgex-er52`). Dataset terms and attribution remain those of the original releases; retained source notices are under `provenance/`. This repository does not grant a new dataset license. No software license has been selected on behalf of the authors; the authors should choose one before advertising permissive reuse.

Upload the **contents of this directory** to the repository root, preserving the directory structure. Do not upload the parent local evidence directory or replace the full tree with the old two partial ZIP files.
