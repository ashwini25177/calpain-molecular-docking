# ligands/

AutoDock Vina docking output files (PDBQT format) against the Calpain-1 (8GX3) receptor. Each file contains up to 10 ranked poses (MODEL 1 = best pose).

| File | Ligand | Role | Best Score (kcal/mol) |
|---|---|---|---|
| MDL28170_docked.pdbqt | MDL-28170 | Known inhibitor (reference/anchor compound) | -6.7 |
| CID_2974355_docked.pdbqt | CID 2974355 | Discovery set — top hit / champion | -7.2 |
| CID_6915837_docked.pdbqt | CID 6915837 | Discovery set — second hit (later ruled out by MD, see results_summary.md) | -6.9 to -7.1 (report vs. slides differ — see root results_summary.md) |

Grid box centered on the catalytic triad (Cys84–His241–Asn265): center (-27.738, 15.174, 36.564), 22×22×22 Å, exhaustiveness 8, 10 modes.
