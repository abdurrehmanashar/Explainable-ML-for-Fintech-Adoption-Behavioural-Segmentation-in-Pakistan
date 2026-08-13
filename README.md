# Beyond Access: Explainable Machine Learning Reveals a Connected but Unengaged Population in Pakistan's Digital Financial System

**Abdur-Rehman Ashar¹, Umema Ashar²**
¹ Beaconhouse School System PTC, Gujranwala, Pakistan
² KDD Lab, FAST-NUCES, Islamabad, Pakistan

[📄 Read the Paper](./paper/beyond-access-digital-financial-system.pdf) ·
[📊 Data Source](https://microdata.worldbank.org/index.php/catalog/7961) ·
[📚 Citation](#citation)


## Headline finding

Using individual-level data from the Global Findex Database 2025 (Pakistan, n=1,000), this study finds that **44.3% of the sample belongs to a segment we call "Connected but Unengaged"**: near-universal mobile phone ownership (98.0%) and majority internet use (62.3%), yet two-thirds (66.8%) remain formally excluded from the financial system — nearly ten times the directly-measured dormant-account rate (6.6%).

This segment is not an artifact of a single method. It emerges independently from three unrelated analytical routes:
1. **Unsupervised clustering** (k-means, blind to the adoption label) — finds this segment on its own
2. **Model error analysis** — the demographic profile of people the classifier mistakenly predicts as "digitally engaged" matches this same segment
3. **Feature-group ablation** — removing behavioural features costs far more predictive power than removing access features, which actually *improves* it

Gender, not urban/rural status, is the strongest structural predictor of segment membership (Cramer's V = 0.807). All findings are associational; the cross-sectional data does not support causal claims.

## Repository structure

```
.
├── README.md
├── LICENSE
├── CITATION.cff
├── requirements.txt
├── .gitignore
│
├── paper/
│   ├── fintech_adoption_pakistan.tex     # Single-file IEEE-format source, inline bibliography
│   └── figures/                          # All figures referenced by the .tex file (see below)
│
├── data/
│   └── README.md                         # How to obtain the Findex microdata (not redistributed here)
│
├── 01_load_and_construct_target.py       # Data cleaning, Adoption Tier target construction
├── 02_model_comparison.py                # Logistic Regression / Decision Tree / RF / XGBoost + SHAP
├── 03_segmentation.py                    # k-means segmentation, PCA visualization
├── 04_statistical_tests.py               # Chi-square tests, Wilson confidence intervals
├── 05_model_diagnostics.py               # Hyperparameter tuning, robustness, calibration, ablation, error analysis
├── 06_cluster_validation.py              # Bootstrap stability, algorithm cross-check, k=2 vs k=3
└── 07_shap_summary_tuned.py              # Regenerates Fig. 5 from the tuned model (see note below)
```

## Reproducing the analysis

Requires Python 3.9+.

```bash
pip install -r requirements.txt
```

1. Download the Findex Pakistan 2024 microdata — see `data/README.md` for instructions (not redistributed in this repo; World Bank microdata terms require direct download).
2. Run the pipeline in order:

```bash
python 01_load_and_construct_target.py
python 02_model_comparison.py
python 03_segmentation.py
python 04_statistical_tests.py
python 05_model_diagnostics.py      # requires 02's output
python 06_cluster_validation.py     # requires 03's output
python 07_shap_summary_tuned.py     # regenerates the corrected Fig. 5 (see note below)
```

Each script reads the output of an earlier script and writes its own CSV/PNG outputs. Move the generated PNGs into `paper/figures/` before compiling the paper.

**Note on Fig. 5**: an earlier draft's SHAP summary figure was generated from the *default* (untuned) Random Forest, which did not match the SHAP ranking reported in the text and Table XI (both computed from the *tuned* model). `07_shap_summary_tuned.py` regenerates this figure correctly from the tuned model and includes a built-in check against the paper's reported ranking order.

## Building the paper

`paper/fintech_adoption_pakistan.tex` is a self-contained IEEE conference-format file (`IEEEtran.cls`) with the bibliography inline — no separate `.bib` file.

```bash
cd paper
pdflatex fintech_adoption_pakistan.tex
pdflatex fintech_adoption_pakistan.tex   # second pass resolves citations and cross-references
```

## Results summary

| Model | Accuracy | Macro F1 |
|---|---|---|
| XGBoost | 0.800 | 0.481 |
| Random Forest (default) | 0.708 | 0.486 |
| **Random Forest (tuned)** | — | **0.494** |
| Decision Tree | 0.488 | 0.424 |
| Logistic Regression | 0.420 | 0.392 |

Repeated cross-validation (50 folds): mean macro F1 = 0.480 (95% CI: 0.447–0.521).

| Cluster | Size | % Female | % Excluded |
|---|---|---|---|
| Digitally Excluded Women | 43.1% | 96.3% | 89.3% |
| Connected but Unengaged | 44.3% | 16.3% | 66.8% |
| Advanced Adopters | 12.6% | 10.3% | 0.8% |

Cluster stability: bootstrap ARI = 0.904 (95% CI: 0.847–0.982); ARI vs. Ward agglomerative clustering = 0.740.

## Key methodological notes

- **Leakage audit**: an initial specification retaining `account`, `dig_account`, `anydigpayment`, `saved`, `fin22a`, `account_fin`, `account_mob` produced spurious near-perfect accuracy. These variables directly define the target and were removed; the failed initial run is documented in the paper (Section III-C) rather than omitted, for methodological transparency.
- **Unweighted analysis**: all reported proportions describe the n=1,000 analytical sample as collected, without applying Findex's survey design weights. See Section III-A and the Limitations section for the full discussion — this is a genuine constraint on generalizing these proportions to the national population without reweighting.
- **Modelling objective**: the supervised classifier is used as an explainability/diagnostic instrument (SHAP, permutation importance, calibration, error profiling), not as a deployment-ready individual-level classifier. See Section III-D.

## Ethical considerations

Summarized from Section VI of the paper (see the paper for full discussion):
- Raw subgroup accuracy is a misleading fairness metric here, since gender is strongly entangled with outcome — subgroup-level macro F1 is recommended instead.
- This model is not recommended for individual-level credit or eligibility decisions; its intended use is aggregate, population-level policy targeting.
- Segment labels should not be treated as immutable individual characteristics or used to deny services.
- Predicted probabilities for three of the four adoption tiers are poorly calibrated and should not be used in downstream resource-allocation formulas without recalibration.

## Citation

```bibtex
@unpublished{ashar2026beyondaccess,
  author = {Ashar, Abdur-Rehman and Ashar, Umema},
  title  = {Beyond Access: Explainable Machine Learning Reveals a Connected but Unengaged Population in Pakistan's Digital Financial System},
  year   = {2026},
  note   = {Preprint},
  url    = {https://github.com/abdurrehmanashar/Explainable-ML-for-Fintech-Adoption-Behavioural-Segmentation-in-Pakistan}
}
```

See also `CITATION.cff` for machine-readable citation metadata (GitHub renders a "Cite this repository" button automatically when this file is present).

## Data citation

World Bank. 2025. *Global Findex Database 2025, Pakistan.* Reference: PAK_2024_FINDEX_v02_M. World Bank Microdata Library. https://microdata.worldbank.org/index.php/catalog/7961

## License

Code in this repository is released under the MIT License (see `LICENSE`). The paper text and figures are not covered by this license — see `paper/README.md` or contact the authors regarding reuse of the manuscript itself.
