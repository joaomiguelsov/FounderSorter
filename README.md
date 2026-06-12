# Founder Analysis 3 (FA3)

A Python desktop application for phylogenetic founder analysis, computing and visualising the probability that lineages in a tree were introduced by migration events at specific dates, distinguishing two founder types.

Implements the methodology described in **Vieira et al. (2019)** *(PACBB, AISC 1005, pp. 121–128, DOI: [10.1007/978-3-030-23873-5_15](https://doi.org/10.1007/978-3-030-23873-5_15))*.

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

### Option A — Windows Executable (recommended)

No Python installation required.

1. Download `FA3.exe` and `FA3Help.md` from the [latest release](../../releases/latest)
2. Place both files in the same folder, e.g. `C:\FounderAnalysis\`
3. Double-click `FA3.exe` to launch

> Requires Windows 11 (64-bit).

---

### Option B — Run from Source

#### 1. Download from GitHub (recommended)

1. Go to the repository page:
   https://github.com/joaomiguelsov/FounderSorter/tree/main

2. Download the source files and place them all in the same folder, e.g. `C:\FounderSorter\`

```
FounderSorter/
├── FA3_Version44.py   ← main application
├── tree3.py
├── plots3.py
├── toss.py
├── FA3Help.md
└── favicon.ico        (optional — application icon)
```

#### 2. Clone with Git

```bash
git clone https://github.com/joaomiguelsov/FounderSorter.git
cd FounderSorter
```

#### 3. Install Python dependencies

```bash
pip install matplotlib openpyxl
```

Anaconda users: use your Anaconda environment and run `conda install matplotlib openpyxl` or `pip install` inside the activated environment.

#### 4. Run the application

```bash
python FA3_Version44.py
```

On Windows you can also double-click `FA3_Version44.py` if `.py` files are associated with Python, or use:

```bash
# Windows — explicit path example
python C:\FounderSorter\FA3_Version44.py
```

The GUI window will open immediately. No further configuration is needed.

---

### Option C — Build a standalone Windows executable

If you want to build the executable yourself from source:

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

## Features

- Load and inspect annotated XML phylogenetic trees with full node/leaf statistics
- Classify leaves as sources or sinks via an importable type file
- Compute migration probabilities across a date range or custom date set, for both founder types simultaneously
- Interactive five-tab tree viewer with sortable columns and keyboard navigation
- Three publication-ready plots with captions, legends, and embedded toolbar
- Export results as Newick, Excel, TSV, or leaf list
- Progress windows with elapsed time and ETA for all long-running operations

---

## File Structure

```
├── FA3_Version44.py   # Main application — GUI, menus, threading, progress bars
├── tree3.py           # Tree data model — XML parsing, node stats, migrationProbs()
├── plots3.py          # Plotting module — rangeProb(), barProb(), stackProb()
├── toss.py            # Excel export utility (SS class)
├── FA3Help.md         # In-app help documentation
└── favicon.ico        # Application icon (optional)
```

---

## Usage

### Workflow

1. **File → Open XML tree file** — load an annotated phylogenetic tree (`.xml`)
2. **File → Import source-sink file** — classify leaves as sources or sinks via a `.txt` file
3. **Migrations → Compute using range** or **Compute using custom dates** — set migration dates, mutation rate, and options
4. **Plots menu** — open any of the three visualisations once migration probabilities are computed

Full documentation is available in the in-app **Help → README** tab.

---

## Input Formats

### XML Tree File

Annotated phylogenetic tree formatted as in the [PhyloTree](http://www.phylotree.org) platform (van Oven & Kayser, 2009), with per-node attributes including mutations, haplogroups, node IDs, and pre-computed statistics (Rho, age, confidence interval).

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
| Tree information | All nodes and leaves: mutations, leaf count, Rho, std err, age, confidence interval |
| ƒ¹ statistics | mutations, ƒ¹ leaves, Rho, std err |
| ƒ² statistics | mutations, ƒ² leaves, Rho, std err, eligibility |
| ƒ¹ founder analysis | Per-node migration probabilities at each date (ƒ¹) |
| ƒ² founder analysis | Per-node migration probabilities at each date (ƒ²) |

---

## Plots

### Plot 1 — Migration Scan (Probabilistic Distribution — Range)
Smooth line plot of the mean proportion of lineages explained by migration across a continuous date range, for both ƒ¹ and ƒ².

### Plot 2 — Migration Model (Probabilistic Distribution — Bars)
Bar chart of mean ± s.d. per migration date. ƒ¹ bars are solid; ƒ² bars are hatched. Includes dual legend (migration dates + founder type).

### Plot 3 — Individual Proportions
Horizontal stacked bar chart showing the per-node breakdown of migration proportions at each date. Two side-by-side panels for ƒ¹ and ƒ². Bars are optionally scaled proportional to sample size.

---

## Founder Criteria

| Criterion | Definition |
|---|---|
| **f1** | At least one derived branch of the founder haplotype is detected in the source population |
| **f2** | At least two derived branches are detected in the source population (stricter) |

---

## Computation Parameters

| Parameter | Description |
|---|---|
| Migration date range | Specify earliest date, latest date, and step interval |
| Custom dates | Comma-separated list of specific migration dates |
| Mutation rate | Per-lineage mutation rate (e.g. 2600) |
| Use effective number | Toggle effective population size correction |
| Remove singletons | Exclude singleton nodes from analysis |

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

## Case Study

Haplogroup **B4a1a1** (Polynesian motif) was used to validate the tool:

- **Source**: Near Oceania (including New Guinea)
- **Sink**: Remote Oceania (Vanuatu to Hawaii)
- **Tree**: 605 nodes, 2015 leaves (754 source, 332 sink)
- **Results**: Migration scan peaks at ~3400–3500 years ago and ~900 years ago; two-migration model assigns ~80% of lineages to the first migration

---

## Notes

- The application uses background threads for XML parsing and migration computation to keep the GUI responsive
- All plots embed a matplotlib NavigationToolbar for pan, zoom, and save-to-file
- The `plots3.py` module is independent and can be imported outside the GUI for scripted use

---

## Reference

> Vieira D., Almeida M., Richards M.B., Soares P. (2019). *An Efficient and User-Friendly Implementation of the Founder Analysis Methodology.* PACBB 2019, AISC 1005, pp. 121–128. https://doi.org/10.1007/978-3-030-23873-5_15

---


---

## Authors

Specify authors / institution here.
