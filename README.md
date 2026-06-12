# Founder Analysis

> A desktop tool for phylogenetic founder analysis of mitochondrial DNA trees

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows%2011-0078D4?logo=windows&logoColor=white)
![GUI](https://img.shields.io/badge/GUI-Tkinter-orange)
![License](https://img.shields.io/badge/License-MIT-green)

Computes and visualises the probability that lineages in a phylogenetic tree were introduced by migration events at specific dates, distinguishing two founder stringency levels (ƒ¹ and ƒ²).

Implements the methodology described in **Vieira et al. (2019)** — *An Efficient and User-Friendly Implementation of the Founder Analysis Methodology*, PACBB 2019, AISC 1005, pp. 121–128. [https://doi.org/10.1007/978-3-030-23873-5_15](https://doi.org/10.1007/978-3-030-23873-5_15)

---

## Contents

- [Installation](#installation)
  - [Option A — Windows Executable](#option-a--windows-executable-recommended)
  - [Option B — Run from Source](#option-b--run-from-source)
  - [Option C — Build Executable from Source](#option-c--build-executable-from-source)
- [Usage](#usage)
- [Input Formats](#input-formats)
- [Tabs](#tabs)
- [Plots](#plots)
- [Computation Parameters](#computation-parameters)
- [Export Options](#export-options)
- [Case Study](#case-study)
- [File Structure](#file-structure)
- [Authors](#authors)
- [Reference](#reference)

---

## Installation

### Requirements

| Dependency | Version |
|---|---|
| Python | ≥ 3.8 |
| matplotlib | ≥ 3.5 |
| openpyxl | ≥ 3.0 |
| numpy | any |
| tkinter | bundled with CPython |

---

### Option A — Windows Executable (recommended)

No Python installation required.

1. Go to the [**Releases**](../../releases/latest) page
2. Download `FA3.exe` and `FA3Help.md`
3. Place both files in the same folder, e.g. `C:\FounderAnalysis\`
4. Double-click `FA3.exe` to launch

> ⚠️ Requires Windows 11 (64-bit).

---

### Option B — Run from Source

**1. Clone the repository**

```bash
git clone https://github.com/joaomiguelsov/FounderSorter.git
cd FounderSorter
```

Or download the source archive from the [Releases](../../releases/latest) page and extract all files to the same folder:

```
FounderSorter/
├── FA3_Version44.py   ← main application
├── tree3.py
├── plots3.py
├── toss.py
├── FA3Help.md
└── favicon.ico        (optional — window icon)
```

**2. Install dependencies**

```bash
pip install matplotlib openpyxl numpy
```

> Anaconda users: activate your environment and run `conda install matplotlib openpyxl numpy` or use `pip install` within the activated environment.

**3. Run**

```bash
python FA3_Version44.py
```

On Windows you may also double-click `FA3_Version44.py` if `.py` files are associated with Python.

---

### Option C — Build Executable from Source

```bash
pip install pyinstaller

pyinstaller --onefile --windowed --icon=favicon.ico ^
  --name="FounderAnalysis" ^
  --add-data="favicon.ico;." ^
  --add-data="FA3Help.md;." ^
  --hidden-import=tkinter ^
  --hidden-import=tkinter.ttk ^
  --hidden-import=matplotlib.backends.backend_tkagg ^
  --collect-data=matplotlib ^
  --hidden-import=openpyxl ^
  FA3_Version44.py
```

The executable will be created at `dist/FounderAnalysis.exe`.

---

## Usage

The application guides you through four sequential steps shown in the top toolbar:

| Step | Action | Menu |
|---|---|---|
| 1 — Load XML Tree | Load a PhyloTree-formatted `.xml` file | *File → Open XML tree file* |
| 2 — Import Source/Sink | Classify leaves as source or sink via a `.txt` file | *File → Import source-sink file* |
| 3 — Compute Migrations | Set parameters and compute migration probabilities | *Migrations → Compute using range* or *custom dates* |
| 4 — Generate Plots | Visualise results and export figures as PNG | *Plots* menu |

> 📖 Full documentation is available in the in-app **Help → README** tab.

---

## Input Formats

### XML Tree File

A single mtDNA phylogenetic tree formatted as in the [PhyloTree](http://www.phylotree.org) platform (van Oven & Kayser, 2009). Each node must have an `id` attribute and a mutations (`HGs`) attribute with pre-computed statistics (Rho, age, confidence interval).

### Source-Sink File (`.txt`)

Plain-text file mapping leaf IDs to population type, one entry per line:

```
LeafID_1    A
LeafID_2    B
LeafID_3    A
```

| Code | Meaning |
|---|---|
| `A` | Source population |
| `B` | Sink population |
| `U` | Unclassified — excluded from founder analysis |

---

## Tabs

| Tab | Contents |
|---|---|
| **Step 1 — Tree information** | All nodes and leaves: mutations, leaf count, Rho (ρ), std err, age, confidence interval |
| **Step 2 — ƒ¹ statistics** | Mutations, ƒ¹ leaf count, ρ, std err |
| **Step 2 — ƒ² statistics** | Mutations, ƒ² leaf count, ρ, std err, ƒ² eligibility |
| **Step 3 — ƒ¹ founder analysis** | Per-node migration probabilities at each date (founder type 1) |
| **Step 3 — ƒ² founder analysis** | Per-node migration probabilities at each date (founder type 2) |

### Founder Criteria

| Criterion | Definition |
|---|---|
| **ƒ¹** | At least **one** derived branch of the founder haplotype is detected in the source population |
| **ƒ²** | At least **two** derived branches are detected in the source population (stricter) |

---

## Plots

### Plot 1 — Migration Scan
*Plots → Step 4 Plot 1 Migration scan probabilistic distribution*

Continuous line plot of the mean proportion of lineages attributed to each migration date across a date range, for both ƒ¹ (blue) and ƒ² (orange). Shaded bands represent ±1 standard deviation.

### Plot 2 — Migration Model
*Plots → Step 4 Plot 2 Migration model probabilistic distribution bars*

Grouped bar chart of mean ± s.d. per discrete migration date. ƒ¹ bars are solid; ƒ² bars are hatched. Uses a colour-blind-safe Okabe-Ito palette. Includes dual legend for migration dates and founder type.

### Plot 3 — Individual Proportions
*Plots → Step 4 Plot 3 Migration model individual proportions stacked*

Horizontal stacked bar chart showing per-node migration proportions. Two side-by-side panels for ƒ¹ and ƒ². Bars can optionally be scaled proportional to sample size. Automatically downsamples large datasets for readability.

> All plots embed a matplotlib NavigationToolbar for pan, zoom, and PNG export.

---

## Computation Parameters

| Parameter | Description | Default |
|---|---|---|
| Latest date | Most recent migration date (years BP) | 100 |
| Earliest date | Oldest migration date (years BP) | 50000 |
| Interval step | Step size for date range | 100 |
| Custom dates | Comma-separated list of specific dates | — |
| Mutation rate | Per-lineage mutation rate (years/mutation) | 2600 |
| Use effective number | Toggle effective population size correction | Off |
| Remove singletons | Exclude singleton nodes from analysis | Off |

---

## Export Options

| Menu item | Output format |
|---|---|
| *File → Export tree — Newick format* | `.nwk` |
| *File → Export tree — Excel spreadsheet* | `.xlsx` |
| *File → Export sample list (leaves)* | `.txt` |
| *File → Save current table (TSV)* | `.txt` — statistics for the active tab |
| *File → Save all statistics (TSV)* | `.txt` — combined statistics across all tabs |

---

## Case Study

Haplogroup **B4a1a1** (Polynesian motif) was used to validate the tool:

| | |
|---|---|
| **Source** | Near Oceania (including New Guinea) |
| **Sink** | Remote Oceania (Vanuatu to Hawaii) |
| **Tree** | 605 nodes, 2015 leaves (754 source, 332 sink) |
| **Founders** | 31 ƒ¹, 21 ƒ² |
| **Mutation rate** | 2590 years/mutation |
| **Migration scan** | Peaks at 3400–3500 and ~900 years ago |
| **Two-migration model** | ~80% of lineages in first migration, ~20% in second |

---

## File Structure

```
├── FA3_Version44.py   # Main application — GUI, menus, workflow, threading, progress bars
├── tree3.py           # Tree data model — XML parsing, node statistics, migrationProbs()
├── plots3.py          # Plotting module — rangeProb(), barProb(), stackProb()
├── toss.py            # Excel export utility (SS class)
├── FA3Help.md         # In-app help documentation (README + reference paper)
└── favicon.ico        # Application window icon (optional)
```

---

## Authors

- **João Carneiro**
- **Daniel Vieira**
- **Mafalda Almeida**
- **Martin B. Richards**
- **Pedro Soares**
- **Pedro Fernandes**

---

## Reference

> Vieira D., Almeida M., Richards M.B., Soares P. (2019). *An Efficient and User-Friendly Implementation of the Founder Analysis Methodology.* In: Rocha M., Fdez-Riverola F., Mohamad M., Casado-Vara R. (eds) Practical Applications of Computational Biology & Bioinformatics, 13th International Conference. PACBB 2019. Advances in Intelligent Systems and Computing, vol 1005. Springer, Cham. https://doi.org/10.1007/978-3-030-23873-5_15
