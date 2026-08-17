# Data Card — ICCIT 2026 Bangla Dialect Calibration Project

## Sources
- **BD-SHS** (Romim et al., LREC 2022) — https://github.com/naurosromim/hate-speech-dataset-for-Bengali-social-media
  (data hosted on Kaggle: `naurosromim/bdshs`). License: [TODO — confirm on GitHub repo, was MIT for code;
  confirm dataset terms separately].
- **BIDWESH** (Fayaz et al., 2025, arXiv:2507.16183) — https://data.mendeley.com/datasets/bpkrvf882k/1
  License: [TODO — confirm on Mendeley dataset page before publishing].

Both papers must be cited in the final submission (Part 1 §3).

## Preprocessing steps applied (in order)
1. Column auto-detection / standardization to `text`, `label` (label mapped to {0,1}, 1 = hate).
2. Text cleaning: URLs -> `<URL>`, @mentions -> `<MENTION>`, whitespace collapsed.
   **Emoji were deliberately NOT stripped** — some emoji carry hateful meaning on their own in this domain,
   so removing them risks silently discarding label-relevant signal.
3. Deduplication on exact-match normalized (lowercased, whitespace-collapsed) text.
4. Rows with empty text after cleaning, or unmapped/missing labels, were dropped.
5. BD-SHS split: used the official pre-split train/val/test files as shipped.
6. BIDWESH: melted from its wide parallel format (one row per base instance, one column per dialect) into
   long format (one row per dialect instance). Each dialect row was aligned back to a BD-SHS `id` via exact
   normalized-text match against the BIDWESH "Standard Bangla" column; match rate was 99.2%.
   Unmatched rows are kept (they still have a valid `base_id` linking the 3 dialects to each other for
   paired analysis) but have `matched_bdshs_id = NaN`.

## Split sizes and label balance
| split           |     n |   hate_rate |
|:----------------|------:|------------:|
| train           | 40221 |      0.4804 |
| val             |  5028 |      0.4805 |
| test_standard   |  5029 |      0.4804 |
| test_chittagong |  3061 |      0.4943 |
| test_noakhali   |  3061 |      0.4943 |
| test_barishal   |  3061 |      0.4943 |

## Known limitations (see also Role 4's Limitations section)
- Vocabulary-overlap and length statistics are computed on whitespace tokenization only (no Bangla-specific
  tokenizer/stemmer), so numbers are a rough proxy for linguistic distance, not a precise measure.
- Only 3 of Bangladesh's many regional dialects are covered (Chittagong, Noakhali, Barishal), all originating
  as translations of the same base instances per dialect — this is a
  controlled, but small and single-source, distribution-shift test bed.

Generated automatically by `01_data_engineering.ipynb`. Seed = 42.
