# PrudentPool

PrudentPool is a memory-based, evidence-guided exploration method for DQN autonomous driving. During training, the method uses stored state/action hazard evidence to filter exploration actions. The guidance is removed for frozen-policy testing, so evaluation assesses the learned policy.

This repository packages source code, recorded experiment outputs, analysis scripts, tables, figures, configuration snapshots, and a short demonstration clip for **MetaDrive** and **HighwayEnv**. No new training runs are required to inspect the supplied results.

[Watch the PrudentPool demonstration](https://youtu.be/bFadWcHxJ_c).

## Repository contents

| Path | Contents |
|---|---|
| `env/metadrive/` | MetaDrive source, 23-seed evaluation data, analysis, and figures |
| `env/highway/` | HighwayEnv source, evaluation data, analysis, and figures |
| `env/*/00_README_FIRST.md` | Environment-specific evaluation details and result summaries |
| `env/*/02_source_code/` | Policies, baseline trainers, comparison tools, and analysis |
| `env/*/03_configuration/` | Dependencies and environment specifications |
| `env/*/04_data/` | Recorded CSV/JSON experiment outputs |
| `env/*/05_results/` | Tables and publication figures in PDF/PNG format |
| `env/*/06_checksums/` | File integrity manifests and validation records |

The MetaDrive archival policy is `env/metadrive/02_source_code/policies/prudentpool.py`. Its modular launcher is `policies/prudentpool_modular.py`, supported by `policies/prudentpool_components/`. HighwayEnv policies retain the experiment identifiers `Karthikeya27adv23` and `Karthikeya27adv8956`; MetaDrive records retain `Karthikeya27adv8`. These identifiers link outputs to their original runs and are distinct from the public method name **PrudentPool**.

## Inspect the supplied results

Open the environment-specific `00_README_FIRST.md` and the tables/figures under `05_results/`. Recorded outputs are included; trained neural-network weights and training-memory snapshots are not included in this distribution.

The reported comparisons use Epsilon-Greedy DQN, NoisyNet DQN, and DQN with Random Network Distillation (RND). This is an **apples-to-apples comparison within a DQN exploration setting**, rather than evidence of superiority over every safe-RL framework. The HighwayEnv advanced policies additionally access road-vehicle objects for safety evidence; their baseline observation inputs differ in that respect, as documented in `env/highway/02_source_code/README.md`.

Collision RMST measures the restricted time to the recorded vehicle-collision endpoint. Its interpretation depends on how other episode terminations are handled; road departure, arrival, and timeout must be considered alongside collision outcomes. Maximum-step completion is a separate measure and is not a substitute for destination success, route progress, speed, or overall driving utility. See each environment's data dictionary, configuration, and analysis code for the implemented definitions.

## Regenerate analysis without training

Run these commands from the repository root in a virtual environment:

```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install numpy pandas scipy matplotlib
```

Regenerate the MetaDrive tables and figures:

```bash
python -u env/metadrive/02_source_code/analysis/generate_ieee_results.py
# Alternatively on a system with Bash:
bash env/metadrive/02_source_code/runners/regenerate_results.sh
```

Regenerate the HighwayEnv collision-RMST analysis:

```bash
python env/highway/02_source_code/analysis/compute_collision_rmst.py
```

These commands read the supplied logs and overwrite generated result outputs. The HighwayEnv script writes to `env/highway/05_results/recomputed/`. Analysis involves bootstrap resampling and may take time, but does not train driving agents or require a simulator/GPU. After regenerating files, the delivered checksum manifests no longer necessarily describe the changed outputs.

## Validate the modular MetaDrive implementation

From the repository root:

```bash
cd env/metadrive/02_source_code
python -m unittest discover -s tests -v
```

These supplied tests use Python and NumPy to check modular/archival source compatibility, defaults, and memory components. They do not validate new simulator performance.

## Optional simulator installation and training

New training is optional and requires additional dependencies and compute. Review the environment snapshots before installation:

```bash
python -m pip install -r env/highway/03_configuration/requirements.txt
# MetaDrive has a fuller, platform-specific dependency snapshot:
# env/metadrive/03_configuration/requirements.txt
```

For HighwayEnv smoke tests and training examples, follow `env/highway/02_source_code/README.md`. For MetaDrive policy/training entry points, follow `env/metadrive/02_source_code/README.md` and the policy README. Review device settings before running commands that request CUDA. Dependency snapshots describe the original environment and may require adaptation for a different OS, Python version, or accelerator.

## Scope and limitations

This package contains existing DQN experiments. It adds no modern safe-RL baseline runs, PPO/SAC validation, or newly recorded qualitative case studies. PPO and SAC are alternative learning algorithms; testing compatibility with them would require additional implementation and experiments. Claims should remain limited to the included settings and baselines. The included short clip illustrates driving behavior; it is not a systematic case-study evaluation.

## Naming and integrity

The method name is consistently **PrudentPool**, including documentation, code identifiers, data labels, filenames, figure labels, and the demonstration clip. Numerical experiment values and training logic have been preserved. The packaged HighwayEnv analysis paths were corrected to match this directory layout.

Current `SHA256SUMS.txt` manifests describe this rebranded distribution. `original_SHA256SUMS.csv` is a historical source-archive hash record, not a checksum of the current files. Previously generated validation records refer to the recorded experiments; see `REBRANDING_REPORT.md` for the checks performed on this delivery.
