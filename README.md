# High-Performance Data Analytics with Python

Training materials for the **KUKDM 2026** (Konferencja Użytkowników Komputerów Dużej Mocy) hands-on workshop.

**Duration**: 4 hours (2 × 2 h with a break)  
**Platform**: Athena supercomputer (ACK Cyfronet AGH) via JupyterHub  
**Paper**: [Fast silicon detectors as proton therapy beam monitors (arXiv:2508.12451)](https://arxiv.org/abs/2508.12451)

## Prerequisites

- Basic Python knowledge (variables, loops, functions)
- Familiarity with the Linux command line
- JupyterHub account on Athena (provided during the workshop)

## Setup

1. Log into JupyterHub on Athena.
2. Clone this repository:
   ```bash
   git clone https://github.com/grzanka/kukdm2026-hpdap.git
   cd kukdm2026-hpdap
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
4. Open `00_setup.ipynb` and follow the environment verification steps.

### Dask Dashboard Access

To access the Dask dashboard from your laptop, set up an SSH tunnel:

```bash
ssh -N -L <port>:localhost:<port> <username>@athena.cyfronet.pl
```

Replace `<port>` with your assigned dashboard port (see `00_setup.ipynb`).

## Notebooks

| # | Notebook | Time | Description |
|---|----------|------|-------------|
| 0 | `00_setup.ipynb` | 10 min | Environment check, participant port assignment, HPC/HPDA framing |
| 1 | `01_why_not_lists.ipynb` | 15 min | Python lists vs NumPy arrays — memory model and performance |
| 2 | `02_pandas_and_dask_intro.ipynb` | 40 min | Pandas basics, weather data, first Dask DataFrame operations |
| 3 | `03_dask_performance.ipynb` | 40 min | Lazy evaluation, task graphs, Pandas vs Dask benchmarks |
| — | *Break* | 15 min | |
| 4 | `04_scientific_data.ipynb` | 45 min | Parquet scientific data (lv1 + lv2), single-node Dask analysis |
| 5 | `05_multinode.ipynb` | 40 min | SLURMCluster, dask-jobqueue, multi-node scaling experiment |
| 6 | `06_challenge.ipynb` | 15 min | Open-ended final exercise — multi-dataset histogram comparison |

## Data

### Scientific datasets (Parquet)

The scientific data originates from the paper above — LGAD silicon detector measurements
in a proton therapy beam. Two processing levels:

- **lv1** (raw waveforms): high-frequency voltage samples with nanosecond timestamps
- **lv2** (extracted features): peak amplitude, rise time, pulse length, polarity

| Dataset | Path on Athena | Format |
|---------|---------------|--------|
| Scientific lv1 | `/net/pr2/projects/tutorial/2026-04-15-hpda/dataset/lv1/` | Parquet |
| Scientific lv2 | `/net/pr2/projects/tutorial/2026-04-15-hpda/dataset/lv2/` | Parquet |

### Weather data

Meteorological station data from [GIG Katowice](https://meteo.gig.eu/),
licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

| Dataset | Location | Format |
|---------|----------|--------|
| Weather (source) | <https://meteo.gig.eu/archiwum/2025/> | TXT (weekly files, whitespace-separated) |
| Weather (backup on Athena) | `/net/pr2/projects/tutorial/2026-04-15-hpda/meteo/` | TXT / HDF5 |
| Weather (in repo) | [`meteo/`](meteo/) | TXT / HDF5 |

The weather files are included in this repository for offline use.
The original data is updated weekly at the source URL.

### Instructor: Pre-stage weather data

Before the workshop, download the weather data once to the shared path so that
participants do not all download simultaneously:

```bash
# Run on Athena as the tutorial user
mkdir -p /net/pr2/projects/tutorial/2026-04-15-hpda/meteo
cd /net/pr2/projects/tutorial/2026-04-15-hpda/meteo
for m in 01 02 03; do
  for d in $(seq -w 1 31); do
    wget -q "https://meteo.gig.eu/archiwum/2025/${m}/${d}.txt" 2>/dev/null || true
  done
done
```

## License

Code: [GPL-3.0](LICENSE)  
Weather data: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (source: [meteo.gig.eu](https://meteo.gig.eu/))
