# Results Summary — Calpain Docking Study

## Binding Affinity Comparison

| Ligand | PubChem CID / ID | Role | Best Vina Score (kcal/mol) | Complex File |
|---|---|---|---|---|
| MDL28170 | Calpain Inhibitor III (ALLN) | Reference/standard inhibitor | -6.7 | `complexes/MDL28170_Complex_3.pdb` |
| Candidate 1 | CID 2974355 | Test compound | **-7.2** | `complexes/Complex_2.pdb` |
| Candidate 2 | CID 6915837 | Test compound | -6.9 | `complexes/Complex_1.pdb` |

*(Lower/more negative Vina score = stronger predicted binding affinity)*

## Interpretation

- Both candidate compounds (CID 2974355 and CID 6915837) show binding affinities comparable to or better than the reference inhibitor MDL28170.
- **CID 2974355** shows the strongest predicted binding affinity (-7.2 kcal/mol), outperforming the reference compound MDL28170 (-6.7 kcal/mol) by 0.5 kcal/mol.
- Binding-site interaction diagrams (see `interaction_analysis/images/`) show the candidate ligands occupying the same pocket region as the reference inhibitor, with contacts to residues such as Thr179 and Ser22 (see individual images for full residue-level detail).

> These are computational docking predictions only. Confirmatory work (e.g., molecular dynamics simulations, binding free-energy calculations such as MM-GBSA, and/or in-vitro assays) is recommended before drawing biological conclusions.
