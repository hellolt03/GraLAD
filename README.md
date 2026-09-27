# LogGTC

## Repository Structure

```text
.
|-- main.py                          # Command-line entry point and evaluation
|-- model.py                         # Ridge-VAR model and anomaly scoring
|-- count_series_preprocessing.py   # Log-to-count-series conversion
|-- sequence_preprocessing.py       # Template normalization and clustering
|-- anomaly_detector.py             # Thresholding and metric utilities
|-- requirements.txt
|-- configs/
|   `-- main_results.yaml            # Configuration for the paper's main results
|-- dataset/                         # Raw datasets (not tracked by Git)
`-- results/                         # Generated caches and outputs
```

## Requirements

The released code was checked with Python 3.13.9. Create an isolated environment and install the dependencies:

```bash
python -m venv .venv
```

PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```


## Datasets

The paper experiments use three labeled system-log datasets: BGL, Thunderbird, and Spirit.

### BGL and Thunderbird

Download **BGL** and **Thunderbird** from the official Loghub Zenodo record:

- https://zenodo.org/records/8196385

Download `BGL.zip` and `Thunderbird.tar.gz`, extract them, and arrange the files as follows:

```text
dataset/
|-- BGL/
|   `-- BGL.log
`-- Thunderbird/
    `-- Thunderbird.log
```


### Spirit

Download **Spirit** from the following Zenodo record:

- https://zenodo.org/records/5176176

Place it in the following location:

```text
dataset/
`-- Spirit/
    `-- Spirit.log
```


### Expected input format

Each input line must begin with a label followed by a numeric timestamp:

```text
<label> <timestamp> <remaining log fields and message>
```

The label `-` denotes a normal log entry; every other label is treated as anomalous. The loader infers where the message body starts for BGL, Thunderbird, and Spirit from the file path.

### Dataset References

When using these datasets, please cite the original supercomputer-log study:

1. Adam J. Oliner and Jon Stearley. "What Supercomputers Say: A Study of Five System Logs." *37th Annual IEEE/IFIP International Conference on Dependable Systems and Networks (DSN'07)*, pp. 575-584, 2007. https://doi.org/10.1109/DSN.2007.103

The source records also request citation of the Loghub paper. Please refer to the official repository as well: https://github.com/logpai/loghub

2. Jieming Zhu, Shilin He, Pinjia He, Jinyang Liu, and Michael R. Lyu. "Loghub: A Large Collection of System Log Datasets for AI-driven Log Analytics." *2023 IEEE 34th International Symposium on Software Reliability Engineering (ISSRE)*, pp. 355-366, 2023. https://doi.org/10.1109/ISSRE59848.2023.00071 (Earlier version: https://arxiv.org/abs/2008.06448)


## How to Run

Run one source-to-target experiment with the main-results configuration:

```powershell
python main.py `
  --config configs/main_results.yaml `
  --source_data dataset/Thunderbird/Thunderbird.log `
  --target_data dataset/BGL/BGL.log
```

Use the same command for the other five source-to-target directions by replacing `--source_data` and `--target_data`. Generated preprocessing caches, score tables, models, and summaries are written under `results/`.
