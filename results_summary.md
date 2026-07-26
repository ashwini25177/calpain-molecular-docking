# Results Summary — Calpain-1 (8GX3) Virtual Screening

## 1. Docking Results (Full Screen, from project report)

Ligand Efficiency (LE) = |ΔG| / NHA (number of heavy atoms).

| Ligand | ΔG (kcal/mol) | NHA | LE | Type |
|---|---|---|---|---|
| CID-2974355 | **-7.2** | 25 | 0.288 | Discovery — **Champion** |
| CID-15308693 | -7.1 | 27 | 0.263 | Discovery |
| CID-275591 | -6.9 | 30 | 0.230 | Discovery |
| CID-6915837 | -6.9 | 28 | 0.246 | Discovery |
| MDL-28170 | -6.7 | 28 | 0.239 | Known inhibitor (reference) |
| E64d | -6.2 | 29 | 0.214 | Known inhibitor |
| ALLN | -5.6 | 21 | 0.267 | Known inhibitor |
| Calpeptin | -5.6 | 19 | 0.295 | Known inhibitor |
**Key finding (report):** The top novel candidates (CID-2974355, CID-15308693) outperform all known inhibitors by 0.5–1.6 kcal/mol.

> **Note on discrepancy:** the presentation slide deck lists CID-6915837 at ΔG = -7.1 kcal/mol (NHA 29, LE 0.245) rather than -6.9/28/0.246 as in the written report — likely two slightly different scoring runs or a rounding/typo between the two documents. Worth reconciling against the original `docking_LE_summary.csv` before this becomes the citable number.

## 2. Molecular Dynamics — CID-2974355 vs CID-6915837 (20 ns, GROMACS)

Only the top two docking hits were carried forward to MD, using AMBER99SB-ILDN with ACPYPE-generated ligand topologies (GAFF/Gasteiger charges).

| Metric | CID-2974355 | CID-6915837 | Verdict |
|---|---|---|---|
| Protein RMSD (mean) | 1.34 Å | 1.30 Å | Both stable |
| Ligand RMSD (mean) | 1.34 Å | 2.70 Å | CID-2974355 wins |
| Ligand RMSD (max) | 1.73 Å | 4.13 Å | CID-2974355 wins |
| H-bond occupancy | Intermittent, improving | Near zero | CID-2974355 wins |
| Radius of gyration (protein) | 21.30 Å (stable) | 21.19 Å (stable) | Equal |
| Equilibration time | ~1 ns | Never equilibrated | CID-2974355 wins |
| **Overall verdict** | **Strong candidate** | Unstable binder | **CID-2974355** |

MD protocol: Steepest-descent energy minimization (Fmax < 1000 kJ/mol/nm, converged in ~733 steps) → 100 ps NVT (300 K, V-rescale) → 100 ps NPT (300 K/1 bar, Parrinello-Rahman) → 20 ns production MD (dt = 2 fs, PME electrostatics, 3.9 ns/day throughput).

> **Not yet in this repo:** raw MD trajectory files (`.xtc`, `.tpr`, `.gro`) and the RMSD/RMSF/Rg/H-bond plots referenced above. These live in `MD_CID_2974355/` and `MD_CID_6915837/` on the compute server — see "Next Steps" below.

## 3. ADMET Profile — Top Candidate (CID-2974355), via SwissADME

| Category | Property | Value |
|---|---|---|
| Basic Info | Molecular Weight | 338.40 g/mol |
| Basic Info | Formula | C20H22N2O3 |
| Physicochemical | H-bond Donors | 2 |
| Physicochemical | H-bond Acceptors | 3 |
| Physicochemical | TPSA | 67.43 Å² |
| Physicochemical | Rotatable Bonds | 10 |
| Lipophilicity | Consensus Log P | 2.61 |
| Solubility | Log S (ESOL) | -3.19 (Soluble) |
| Pharmacokinetics | GI Absorption | High |
| Pharmacokinetics | BBB Permeability | Yes |
| Drug-likeness | Lipinski Rule | 0 violations |
| Drug-likeness | Bioavailability Score | 0.55 |
| Med. Chemistry | PAINS Alerts | 0 |
| Med. Chemistry | Synthetic Accessibility | 3.15 (Feasible) |

## 4. Comparison with Reference Paper

| Aspect | Paper (Glide/Schrödinger) | This Project (Open Tools) |
|---|---|---|
| Protein | Calpain-1 (1KXR, rat) | Calpain-1 (8GX3, human) |
| Screening strategy | Pharmacophore + Glide docking | Structure-based AutoDock Vina |
| Scoring function | G-score (Glide) | Vina affinity (kcal/mol) |
| Drug-likeness filter | QikProp | SwissADME / Lipinski RoF |
| MD simulation | Not performed | 20 ns GROMACS |
| Reproducibility | Proprietary (license required) | Fully open-source |
| Top hit | Pharmacophore matches | CID-2974355 (-7.2 kcal/mol, LE 0.288) |

## 5. Conclusions (from report)

1. **CID-2974355 is the top candidate** — highest binding affinity (-7.2 kcal/mol), strong ligand efficiency (0.288), zero PAINS alerts, and MD-confirmed stability (ligand RMSD 1.34 Å over 20 ns).
2. The discovery set outperforms known inhibitors — top CIDs bind 0.5–1.6 kcal/mol stronger than ALLN and Calpeptin against human Calpain-1.
3. NVT/NPT equilibration confirms a well-behaved system: temperature stabilizes at 300 K within 1 ns, density converges to ~1032 kg/m³.
4. **CID-6915837 was ruled out** by MD — poor ligand stability (RMSD 2.7 Å mean, 4.1 Å max) and near-zero H-bond occupancy over 20 ns, despite a competitive docking score.

## 6. Future Work (from report)
- Extend MD to 100 ns for CID-2974355 to confirm long-term stability.
- MM-GBSA/MM-PBSA re-scoring of binding free energy.
- Hydrogen-bond occupancy analysis to pin down key catalytic-cleft residues.
- ADMET cross-validation with ADMETLab or pkCSM.
- In-vitro validation (Calpain-1 activity assay, IC50).

---

## Next Steps for This Repository
To bring the repo fully in line with the report, consider adding:
- `docking_LE_summary.csv` and docked/complex files for the remaining 5 ligands (CID-15308693, CID-275591, E64d, ALLN, Calpeptin)
- An `md_simulation/` folder with `MD_CID_2974355/` and `MD_CID_6915837/` trajectory outputs and plots (RMSD, RMSF, Rg, H-bond)
- `config.txt` (Vina grid config) and the `docking_LE_summary.csv` for full reproducibility
- The ADMET/SwissADME export and the ligand preparation scripts (`obabel` batch commands)
