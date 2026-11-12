# div_free_rbf

## Install on Ubuntu

The included wheel targets **CPython 3.12, Linux x86-64** (Intel/AMD), with
manylinux2014 / glibc 2.17+ compatibility verified by auditwheel. ARM, Windows,
macOS, and other Python versions need separate builds. Older Ubuntu releases
may need Python 3.12 installed separately.

**Choose one installation method below. These are alternatives, do not run both.**
Run commands from the repository root.

### Option A: Create the Conda environment

Create and activate the environment:

```bash
conda env create -f environment.yml
conda activate div_free_rbf
```

This installs Python 3.12, the compiled fitter, its dependencies, and the notebook
tools. No separate pip installation step is needed. Conda uses the same
`requirements.txt` as the single-command pip installation.

### Option B: Use an existing Python 3.12 environment

Activate your existing environment, then install everything with:

```bash
python -m pip install -r requirements.txt
```

This pip command is an alternative to Option A, not an additional step after it.
Run it from the repository root so the local wheel path resolves.
Dependency versions are included directly in `requirements.txt`; no separate
constraints file is needed. Cython, GCC, and the private source checkout are not
needed to compile the fitter because the wheel is already compiled.

### Register the notebook kernel (either option)

After either installation method, keep the chosen environment activated and
register its Python interpreter with Jupyter once. This registers a Jupyter kernel for
the same environment, it does not create another environment:

```bash
python -m ipykernel install --user --name div_free_rbf --display-name "div_free_rbf"
```

## Configure and run

Edit the **single user configuration file**, [`default_config.json`](default_config.json).
The CLI and Python API automatically read this file when run from the distribution
folder or any of its subfolders, including `demo_data/`. Every run reloads it,
so edits take effect without rebuilding or reinstalling. The wheel contains no
JSON configuration file and never falls back to an installed-package config.
If the external file cannot be found, a clear error is raised.

`--config PATH` and `RBF_FIT_CONFIG` remain optional overrides when running from
another location. The notebook explicitly loads this same root file.

```bash
div-free-rbf --fitacf demo_data/fitacf/20220410.0401.00.bks.fitacf3.bz2 \
  --map2 demo_data/map2/20220410.north.map2.bz2 \
  --time 2022-04-10T05:00:00Z \
  --output outputs/fitted.csv
```

Supply your own files and time. The reader checks filename/record dates and the
MAP2 time match before coordinate conversion, selects the nearest scan per
radar, and processes observations in memory without intermediate disk caching.
Output is CSV or explicitly requested Parquet, with a companion run-settings JSON.

Binning controls in `fitting` default to:

```json
"dff_binning": true,
"dff_bin_size_degrees": {"mlat": 0.5, "mlon": 1.0}
```

These are occupied spatial bins, not a filled uniform MLAT/MLT grid.

```python
from rbf_fit import fit_files

result = fit_files(
    ["demo_data/fitacf/my_radar.fitacf.bz2"],
    "demo_data/map2/my_event.map2.bz2",
    "2022-04-10T05:00:00Z",
)
velocities = result.data
```

## Notebook

Run `jupyter lab demo.ipynb` and select the **div_free_rbf**
kernel. The notebook kernel name and displayed name are both `div_free_rbf`.
For Option A, this kernel runs Python from the Conda environment of the same name.
Edit the fitacf/MAP2 glob patterns and UTC time, then run cells in order.
The notebook includes LOS versus DFF and before/after quality-filter comparisons,
with signed red–blue LOS coloring and the current 18–00 MLT sector view.