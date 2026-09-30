# Source Code

The authoritative artifact-analysis entry point is:

```bash
python -u analysis/generate_ieee_results.py
```

when run from this directory, or:

```bash
bash runners/regenerate_results.sh
```

from any directory.

`policies/prudentpool.py` is the archival, rebranded policy source from the supplied
complete experiment archive. The paper calls this method **PrudentPool**; the
source and raw experiment files use the internal identifier
`Karthikeya27adv8`.

`policies/prudentpool_components/` is a commented, reusable modular form of that
policy. `policies/prudentpool_modular.py` provides the same command-line
contract. The architecture and verification strategy are documented in
`../01_documentation/MODULAR_POLICY_ARCHITECTURE.md`.

Run the source-only compatibility tests with:

```bash
python -m unittest discover -s tests -v
```

`training/train_canonical_baselines.py` implements the three packaged
baselines. The two `compare_*` modules and
`runners/run_seed_policy_comparison.py` are the original comparison utilities.
`analysis/generate_ieee_results.py` is the package-level program used to
validate all 23 seeds and generate the delivered two-metric results.
