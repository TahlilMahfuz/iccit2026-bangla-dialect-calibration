# Role 3 findings — draft for Role 4

**H1 (calibration collapses more than accuracy under dialect shift):**
{'banglabert': 100.0, 'xlmr_base': 66.7} of dialect test sets showed ECE increasing proportionally more than
accuracy dropped, per model. This is broadly consistent with H1.

**H2 (post-hoc calibration recovers safe behavior without dialect data):**
Averaged across dialect test sets, temperature scaling reduced ECE by 0.0126 (absolute)
and Dirichlet calibration by 0.0144, both fit only on standard-Bangla validation data.
See `aurc_table.csv` for whether this translates into a lower area-under-risk-coverage-curve (i.e. whether
abstaining on low-confidence examples actually becomes more effective post-calibration).

**Next steps for Role 4:**
- Use `calibration_metrics_all.csv` and `aurc_table.csv` as the source tables for Results & Discussion.
- Use `reliability_diagrams_*.png` and `risk_coverage_*.png` as the key figures.
- `h1_relative_change_table.csv` has the per-dialect, per-model breakdown needed for the H3 gradient claim
  (Chittagong -> Noakhali -> Barishal) — combine with Role 1's `vocab_overlap.csv` for the linguistic-distance
  argument.

**Caveat:** Numbers are from real model outputs.
