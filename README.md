# Orthographic Structure in Neural Representations of Chinese Characters

Probing whether transformer-based language models encode radical (部首) compositionality in their learned representations of Chinese characters.

## Core Finding

Characters sharing the same Kangxi radical occupy statistically closer regions in embedding space than characters with different radicals — across 6,306 characters and 68 radical groups. The effect is small but robust, surviving bootstrap CI, permutation testing, and radical-size bias checks.

## Models Tested

| Model | Intra-radical | Inter-radical | p-value | Cohen's d | 95% CI |
|---|---|---|---|---|---|
| mBERT | 0.5515 | 0.5353 | 1.65e-08 | 0.14 | [0.011, 0.022] |
| Chinese-BERT (hfl/chinese-bert-wwm-ext) | 0.8572 | 0.8502 | 2.51e-04 | 0.09 | [0.004, 0.011] |

Permutation p = 0.000 for both models.

## Dataset

- **Source**: [Unicode Unihan Database](https://www.unicode.org/Public/UCD/latest/ucd/Unihan.zip) (`kRSUnicode` field)
- **Filter**: CJK Unified Ideographs basic block (U+4E00–U+9FFF), intersected with Chinese-BERT tokenizer vocabulary
- **Final size**: 6,306 characters, 68 radicals (each with ≥20 characters)
- **Saved**: `data/radical_dataset.csv`

## Project Structure

```
chinese_llm_composition/
├── data/
│   ├── unihan/                  # Unihan database files
│   ├── radical_dataset.csv      # Final character→radical dataset
│   ├── radical_summary.csv      # Per-radical statistics
│
├── scripts/
│   ├── build_radical_dataset.py       # Parse Unihan → radical map
│   ├── prepare_final_dataset.py       # Filter to common chars, build CSV
│   ├── extract_embeddings.py          # Initial embedding exploration
│   ├── radical_vs_semantic.py         # Controlled semantic vs radical test
│   └── radical_cohesion_test.py       # Main experiment (t-test, bootstrap, permutation)
├── figures/
│   ├── radical_cohesion_density.png   # Main paper figure (distributions)
│   ├── radical_cohesion_bars.png      # Bar chart comparison
│   ├── permutation_test.png           # Permutation test visualization
│   └── radical_size_bias.png          # Size bias scatter plot
├── results/                           # Canonical saved artifacts (.npy, .csv)
├── paper/                             # Paper drafts
├── requirements.txt
└── README.md
```

## Reproduce

```bash
python -m venv venv
venv\Scripts\activate          # Windows
# source venv/bin/activate     # Mac/Linux

pip install -r requirements.txt

# 0. Download Unihan database
mkdir data\unihan
curl -L -o data/unihan/Unihan.zip https://www.unicode.org/Public/UCD/latest/ucd/Unihan.zip
cd data/unihan && unzip Unihan.zip && cd ../..

# 1. Build dataset (parses Unihan_IRGSources.txt)
python scripts/build_radical_dataset.py
python scripts/prepare_final_dataset.py

# 2. Run main experiment (~2 min on CPU, downloads models on first run)
python scripts/radical_cohesion_test.py
```

## Statistical Methods

- **Cosine similarity** between mean-pooled character embeddings
- **Welch's t-test** (intra- vs inter-radical pair distributions)
- **Cohen's d** effect size
- **Bootstrap** 95% confidence intervals (1,000 resamples)
- **Permutation test** (1,000 label shuffles) — assumption-free significance
- **Spearman correlation** between radical group size and cohesion score (bias check)

## License

Research use. Dataset derived from Unicode Unihan (see Unicode terms of use).
