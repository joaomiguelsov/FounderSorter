# Founder Analysis (FA3)

[![Python](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![GitHub](https://img.shields.io/badge/GitHub-FounderSorter-181717?logo=github)](https://github.com/joaomiguelsov/FounderSorter)

A Python desktop application for phylogenetic founder analysis — computing and visualising the probability that lineages in a tree were introduced by migration events at specific dates, distinguishing two founder types.

---

## Installation

### Requirements

| Dependency | Version |
|---|---|
| Python | ≥ 3.8 |
| matplotlib | ≥ 3.5 |
| openpyxl | ≥ 3.0 |
| tkinter | bundled with CPython |

---

### Option A — Download from GitHub (recommended)

The repository provides two compressed source archives. Download and decompress both before running.

**1. Go to the releases / repository page**

```
https://github.com/joaomiguelsov/FounderSorter/tree/main
```

**2. Download the two source archives** from the repository (`.zip` files) and place them in the same folder, e.g. `C:\FounderSorter\`

**3. Decompress both archives** into the same folder — all `.py` files must be in the same directory:

```
FounderSorter/
├── FA3_Version9.py   ← main application
├── tree3.py
├── plots3.py
├── toss.py
└── favicon.ico       (optional — application icon)
```

On Windows you can right-click each `.zip` → **Extract All…** and point both to the same destination folder.

From the command line:

```bash
# Windows (PowerShell)
Expand-Archive -Path source1.zip -DestinationPath C:\FounderSorter
Expand-Archive -Path source2.zip -DestinationPath C:\FounderSorter

# macOS / Linux
unzip source1.zip -d FounderSorter/
unzip source2.zip -d FounderSorter/
```

---

### Option B — Clone with Git

```bash
git clone https://github.com/joaomiguelsov/FounderSorter.git
cd FounderSorter
```

---

### Install Python dependencies

From inside the `FounderSorter` folder:

```bash
pip install matplotlib openpyxl
```

> **Anaconda users:** use your Anaconda environment and run `conda install matplotlib openpyxl` or `pip install` inside the activated environment.

---

### Run the application

```bash
python FA3_Version9.py
```

On Windows you can also double-click `FA3_Version9.py` if `.py` files are associated with Python, or use:

```bash
# Windows — explicit path example
python C:\FounderSorter\FA3_Version9.py
```

The GUI window will open immediately. No further configuration is needed.

---

### Build a standalone Windows executable (optional)

If you want to distribute FA3 without requiring Python on the target machine:

```bash
pip install pyinstaller

pyinstaller --onefile --windowed --icon=favicon.ico ^
  --name="FounderAnalysis" ^
  --add-data="favicon.ico;." ^
  --hidden-import=tkinter ^
  --hidden-import=tkinter.ttk ^
  --hidden-import=matplotlib.backends.backend_tkagg ^
  --collect-data=matplotlib ^
  --hidden-import=openpyxl ^
  FA3_Version9.py
```

The executable will be created at `dist/FounderAnalysis.exe`.

---

## Features

- Load and inspect annotated **XML phylogenetic trees** with full node/leaf statistics
- Classify leaves as **sources** or **sinks** via an importable type file
- Compute **migration probabilities** across a date range or custom date set, for both founder types simultaneously
- Interactive **five-tab tree viewer** with sortable columns and keyboard navigation
- Three **publication-ready plots** with captions, legends, and embedded toolbar
- Export results as **Newick**, **Excel**, **TSV**, or leaf list
- Progress windows with **elapsed time and ETA** for all long-running operations

---

## File Structure

```
├── FA3_Version9.py   # Main application — GUI, menus, threading, progress bars
├── tree3.py          # Tree data model — XML parsing, node stats, migrationProbs()
├── plots3.py         # Plotting module — rangeProb(), barProb(), stackProb()
├── toss.py           # Excel export utility (SS class)
└── favicon.ico       # Application icon (optional)
```

---

## Usage

### Workflow

1. **File → Open XML tree file** — load an annotated phylogenetic tree (`.xml`)
2. **File → Import source-sink file** — classify leaves as sources or sinks via a `.txt` file
3. **Migrations → Compute using range** _or_ **Compute using custom dates** — set migration dates, mutation rate, and options
4. **Plots** menu — open any of the three visualisations once migration probabilities are computed

---

## Input Formats

### XML Tree File
Annotated phylogenetic tree with per-node attributes including mutations, haplogroups, node IDs, and pre-computed statistics (Rho, age, confidence interval).

### Source-Sink Type File (`.txt`)
Plain-text file mapping leaf IDs to founder type:
```
LeafID_1    A
LeafID_2    B
LeafID_3    A
```
`A` = source, `B` = sink. Unclassified leaves are marked `U` and excluded from founder analysis.

---

## Tabs

| Tab | Contents |
|---|---|
| **Tree information** | All nodes and leaves: mutations, leaf count, Rho, std err, age, confidence interval |
| **ƒ¹ statistics** | Source→sink nodes: mutations, ƒ¹ leaves, Rho, std err |
| **ƒ² statistics** | Sink→source nodes: mutations, ƒ² leaves, Rho, std err, eligibility |
| **ƒ¹ founder analysis** | Per-node migration probabilities at each date (ƒ¹) |
| **ƒ² founder analysis** | Per-node migration probabilities at each date (ƒ²) |

---

## Plots

### Probabilistic Distribution — Range
Smooth line plot of the **mean proportion of lineages** explained by migration across a continuous date range, for both ƒ¹ and ƒ².

### Probabilistic Distribution — Bars
Bar chart of **mean ± s.d.** per migration date. ƒ¹ bars are solid; ƒ² bars are the same colour at reduced opacity. Includes dual legend (migration dates + founder type).

### Individual Proportions
Horizontal stacked bar chart showing the **per-node breakdown** of migration proportions at each date. Two side-by-side panels for ƒ¹ and ƒ². Bars are optionally scaled proportional to sample size.

---

## Computation Parameters

| Parameter | Description |
|---|---|
| **Migration date range** | Specify earliest date, latest date, and step interval |
| **Custom dates** | Comma-separated list of specific migration dates |
| **Mutation rate** | Per-lineage mutation rate (e.g. `2600`) |
| **Use effective number** | Toggle effective population size correction |
| **Remove singletons** | Exclude singleton nodes from analysis |

---

## Export Options

| Menu item | Output |
|---|---|
| Export tree — Newick format | `.nwk` file |
| Export tree — Excel spreadsheet | `.xlsx` file |
| Export sample list (leaves) | `.txt` file |
| Save current table (TSV) | Statistics for the active tab |
| Save all statistics (TSV) | Combined statistics across all tabs |

---

## Change Log

See the header of [`plots3.py`](plots3.py) for the full `plots3` change log (v1.0 → v2.4) and [`FA3_Version9.py`](FA3_Version9.py) for the FA3 change log (v8.0 → v8.1).

---

## Notes

- The application uses **background threads** for XML parsing and migration computation to keep the GUI responsive
- All plots embed a **matplotlib NavigationToolbar** for pan, zoom, and save-to-file
- The `plots3.py` module is independent and can be imported outside the GUI for scripted use

---

## License

_Specify your license here (e.g. MIT, GPL-3.0)._

## Authors

_Specify authors / institution here._
