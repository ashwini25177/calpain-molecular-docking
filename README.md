# Comparative In-Silico Analysis of Drug-Like Compounds Against Calpain-1

**Molecular Docking and MD Simulation | Algorithms in Computational Biology — Course Project**
**Group 11:** Ashwini S. Gudekar (MT25177) · Ishika Gupta (MT25180)

This repository contains the structure-based virtual screening results for identifying inhibitors of **Calpain-1** (human, PDB: [8GX3](https://www.rcsb.org/structure/8GX3)), a calcium-activated cysteine protease implicated in neurodegenerative disease, ischemia-reperfusion injury, and cancer. The study is modeled after a published pharmacophoric virtual screening paper (Bhatt et al., *Medicinal Chemistry Research*, 2013), reproducing it with a fully open-source pipeline (AutoDock Vina, GROMACS, SwissADME) in place of the original's licensed tools (Glide, QikProp).

> **Scope note:** This repo currently holds the **docking-stage outputs** (receptor prep, docked poses, complexes, interaction diagrams) for the three ligands with full result files available: MDL-28170 (reference), CID 2974355, and CID 6915837. The full project additionally includes a 28-compound discovery screen and 20 ns GROMACS MD simulations for the top two hits — summarized below from the project report, with a note on what to add once those raw files are ready.

---

## Protein Target

| Property | Value |
|---|---|
| Target | Calpain-1 catalytic subunit (human), UniProt P07384 |
| PDB ID | [8GX3](https://www.rcsb.org/structure/8GX3) — crystal structure in complex with inhibitor 14c |
| Resolution | 1.99 Å (X-ray) |
| Chain used | Chain A, residues 32–358 (327 residues processed) |
| Co-factors | 2 × Ca²⁺ ions (retained) |
| Catalytic triad | Cys84 – His241 – Asn265 (grid box centered here) |
| Expression system | *E. coli* BL21(DE3) |

## Docking Grid (AutoDock Vina)

| Parameter | Value |
|---|---|
| Center (X, Y, Z) | -27.738, 15.174, 36.564 Å |
| Box size | 22 × 22 × 22 Å |
| Exhaustiveness | 8 |
| Num. modes | 10 (top poses within 4 kcal/mol of best retained) |

*(The project slide deck lists a slightly different box size (21×21×15 Å) and exhaustiveness (16) on the pipeline overview slide — worth reconciling against your actual `config.txt` before finalizing, since the written report and slides don't fully agree on these two values.)*

---

## Repository Structure

```
calpain-molecular-docking/
├── receptor/                 # Prepared 8GX3 receptor (Ca2+ retained, no crystal ligand)
├── ligands/                  # Vina docking output for 3 ligands
├── complexes/                # Receptor + top-pose ligand, merged for visualization
├── interaction_analysis/     # 2D/3D interaction diagrams and DSV session
└── results_summary.md        # Full docking + MD results tables
```

### 1. `receptor/`
| File | Description |
|---|---|
| `protein_vina.pdbqt` | Calpain-1 (8GX3) receptor, cleaned and prepared for Vina: waters and the co-crystallized ligand (KJ0) removed, polar hydrogens added, Gasteiger charges assigned, Ca²⁺ ions retained. |

### 2. `ligands/`
Vina docking output (PDBQT), each with up to 10 ranked poses (Model 1 = best pose).

| File | Ligand | Role | Best ΔG (kcal/mol) |
|---|---|---|---|
| `MDL28170_docked.pdbqt` | MDL-28170 | Known inhibitor — anchor compound for similarity search | -6.7 |
| `CID_2974355_docked.pdbqt` | CID 2974355 | Discovery set — **top hit / champion** | -7.2 |
| `CID_6915837_docked.pdbqt` | CID 6915837 | Discovery set — second hit (later ruled out by MD) | -6.9 to -7.1* |

*Report and slide deck report slightly different values for CID 6915837 (-6.9 vs -7.1 kcal/mol) and its NHA/LE — see `results_summary.md` for both, and confirm against `docking_LE_summary.csv` from the original run.

### 3. `complexes/`
Receptor merged with each ligand's top-ranked pose, for visualization/interaction analysis in Discovery Studio Visualizer.

| File | Complex |
|---|---|
| `MDL28170_Complex_3.pdb` | Calpain-1 + MDL-28170 (reference) |
| `Complex_2.pdb` | Calpain-1 + CID 2974355 (champion) |
| `Complex_1.pdb` | Calpain-1 + CID 6915837 |

### 4. `interaction_analysis/`
| File | Description |
|---|---|
| `MDL28170_Complex_3.dsv` | Discovery Studio Visualizer session file for the MDL-28170 reference complex (structure + computed interaction/conformation data). |
| `images/Compund_2_interaction.png` | Binding-site interaction diagram for CID 2974355. |
| `images/Final_complex_297_2.png` | Rendered complex view highlighting nearby contact residues. |
| `images/Final_complex_CID_6915837_2.png` | Rendered complex view for CID 6915837. |

---

## Pipeline Overview

| Step | Task | Tool(s) |
|---|---|---|
| 1 | Ligand selection | PubChem, BindingDB, ChEMBL |
| 2 | Lipinski/ADMET filtering | SwissADME, RDKit |
| 3 | Ligand preparation | OpenBabel, AutoDock Tools |
| 4 | Protein preparation | AutoDock Tools, Chimera |
| 5 | Binding site / grid definition | AutoDock Vina config |
| 6 | Molecular docking | AutoDock Vina |
| 7 | Post-docking analysis & ranking | Python (pandas, matplotlib), PyMOL |
| 8 | MD simulation | GROMACS 2018, ACPYPE, AMBER99SB-ILDN |

Full ligand dataset: 4 known inhibitors as positive controls (ALLN, Calpeptin, E64d, MDL-28170) and 28 PubChem analogs of MDL-28170, filtered by Lipinski's Rule of Five (MW < 500 Da, HBD ≤ 5, HBA ≤ 10, LogP ≤ 5, TPSA < 140 Å², rotatable bonds < 10) before docking.

See [`results_summary.md`](results_summary.md) for full docking rankings, MD stability metrics, and ADMET profile.

## Requirements to Reproduce / View
- [AutoDock Vina](http://vina.scripps.edu/) 1.1.2+ — docking
- [AutoDockTools / MGLTools](https://ccsb.scripps.edu/mgltools/) 1.5.7 — receptor/ligand preparation
- [OpenBabel](https://openbabel.org/) 3.x — format conversion, Gasteiger charges
- [GROMACS](https://www.gromacs.org/) 2018.1 + [ACPYPE](https://github.com/alanwilter/acpype) — MD simulation
- [SwissADME](http://www.swissadme.ch/) — drug-likeness/ADMET
- [BIOVIA Discovery Studio Visualizer](https://www.3ds.com/products/biovia/discovery-studio/visualization) (free) — to open `.dsv` files and re-render complexes
- PyMOL, Chimera, or VMD can also open the `.pdb`/`.pdbqt` files directly

## Reference
Bhatt M, et al. *Virtual screening based on pharmacophoric features of known calpain inhibitors to identify potent inhibitors of calpain.* Med Chem Res. 2013;23:2445-2455.
