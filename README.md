# Chinese Character Evaluation

Analysis of how multilingual BERT (mBERT) and Chinese-BERT encode **radical structure** in Chinese characters using cosine and Euclidean similarity metrics.

## Overview

This project investigates whether transformer-based language models capture sub-character orthographic information (radicals) in their learned representations of Chinese characters. It compares two models:

- **mBERT** — Google's multilingual BERT
- **Chinese-BERT** — a BERT model pre-trained specifically on Chinese text

## Analysis Pipeline

The Jupyter notebook (`Chinesescores (1).ipynb`) covers:

1. **Data loading & validation** — Loads pre-computed similarity matrices, intra/inter-radical pair distances, bootstrap distributions, and permutation test results.
2. **Embedding visualization** — Spectral embedding + UMAP projections of character representations, colored by radical group.
3. **Intra- vs. inter-radical similarity** — Statistical comparison (bootstrap CIs, permutation tests, Cohen's d) of within-radical vs. across-radical cosine/Euclidean distances.
4. **Per-radical cohesion analysis** — Scatter plots of radical group size vs. cohesion scores.
5. **Semantic control analysis** — Controls for semantic field confounds to isolate orthographic (radical) effects.
6. **Publication-ready figures & tables** — All plots use colorblind-safe palettes and journal-quality formatting.

## Required Data

Place the following 31 files in a `data/` directory:

| Category | Files |
|---|---|
| CSVs | `main_results.csv`, `semantic_control_results.csv`, `radical_dataset.csv` |
| Similarity matrices | `mbert_similarity_matrix.npy`, `chinese_bert_similarity_matrix.npy` |
| Cosine pairs | `*_intra_pairs.npy`, `*_inter_pairs.npy` |
| Euclidean pairs | `*_euclid_intra_pairs.npy`, `*_euclid_inter_pairs.npy` |
| Bootstrap | `*_bootstrap.npy`, `*_euclid_bootstrap.npy` |
| Permutation | `*_permutation_scores.npy`, `*_euclid_permutation_scores.npy` |
| Radical stats | `*_rad_cohesions.npy`, `*_rad_sizes.npy` |
| Semantic control | `*_semantic_control_intra.npy`, `*_semantic_control_cross.npy`, `*_semantic_control_perm.npy` |

## Dependencies

```
numpy
pandas
matplotlib
seaborn
scipy
scikit-learn
umap-learn
```

## Usage

Open the notebook in Jupyter or Google Colab and run all cells sequentially. Figures are saved to `figures/` and summary tables to `paper_tables/`.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/aryan35790jp/chinese_evaluation/blob/main/Chinesescores%20(1).ipynb)
