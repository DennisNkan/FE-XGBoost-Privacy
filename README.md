# FE-XGBoost: Detecting Big Tech Behavioral Profiling Risks in Higher Education

Repository for the research proposal *"FE-XGBoost: A Feature-Engineered Framework for Detecting Big Tech Behavioral Profiling Risks in Higher Education"* (Dennis Agyapong Nkansah, Department of Computer Science, KNUST, 2026).

**Status:** Proposal stage. Ethics application to the KNUST College of Science Ethics Committee (CSEC) is in progress (submitted by 5 July 2026; approval expected by 2 August 2026). No data collection and no code execution will begin before written CSEC approval is received. This repository currently contains project scaffolding only; implementation code will be added following approval, in the phased order described below.

## Project Summary

FE-XGBoost is a feature-engineered variant of XGBoost designed to detect sensitive-attribute inference risk (e.g., inferred gender, location, or behavioral profile) from passive user-interaction logs. It applies three named engineering operations to a standard XGBoost classifier:

- **Fabricate** — a custom asymmetric focal loss (`alpha=0.75`, `gamma=2.0`) that penalizes missed privacy-risk detections (false negatives) more heavily than false positives.
- **Modify** — monotonicity constraints on four engagement-depth features (`session_duration`, `scroll_depth`, `ad_click_count`, `platform_switches`), enforcing non-decreasing risk predictions as engagement deepens.
- **Replace** — the exact-greedy tree-split method is replaced with the `hist` method, capped at `max_depth=4`, to reduce overfitting on a smaller, localized dataset.

The study is grounded in Nissenbaum's Contextual Integrity framework (2004) and evaluates FE-XGBoost against standard XGBoost, LightGBM, Logistic Regression, and Random Forest baselines on primary field data collected from KNUST students, with a public dataset (MyPersonality) used as a supplementary generalizability benchmark.

Full methodology, theoretical grounding, ethics plan, and reproducibility checklist are described in the accompanying research proposal.

## Planned Repository Structure

```
FE-XGBoost-Privacy/
├── README.md
├── requirements.txt
├── data/
│   ├── raw/              # Raw field + public-benchmark data (not committed; see Data Availability)
│   └── processed/        # Cleaned, preprocessed datasets
├── notebooks/
│   ├── 01_baseline_xgboost.ipynb     # Phase 1: locked, unmodified baseline
│   ├── 02_fe_xgboost.ipynb           # Phase 2: F/M/R-engineered model
│   └── 03_comparison_evaluation.ipynb # Phase 3: formal baseline-vs-engineered comparison
├── src/
│   ├── preprocessing.py   # Missing-value handling, encoding, normalization
│   ├── losses.py          # Custom privacy_focal_loss objective (Fabricate)
│   ├── constraints.py     # Monotonicity constraint configuration (Modify)
│   ├── model.py           # FE-XGBoost training wrapper (Replace: hist, max_depth=4)
│   └── evaluation.py      # Metrics, Wilcoxon signed-rank test, Cohen's d
├── results/
│   ├── metrics/           # Per-fold metrics (Accuracy, Macro-F1, AUC-ROC, AUC-PR)
│   └── figures/           # SHAP plots, AUC-PR curves, confusion matrix, ablation table
└── ethics/
    └── consent_materials/ # Information sheet and consent form (English & Twi)
```

This structure is not yet populated with code. It reflects the four-phase workflow defined in the proposal (Phase 0: environment setup → Phase 1: locked baseline → Phase 2: FE-XGBoost engineering → Phase 3: formal comparison).

## Reproducibility

All experiments will use a fixed random seed (`42`) across `random_state`, NumPy's seed, and XGBoost's `seed` parameter. Baseline and FE-XGBoost models will be trained on identical 80/20 stratified data splits, evaluated via 10-fold stratified cross-validation repeated 5 times (50 runs total), with a Wilcoxon signed-rank test (p < 0.05) and Cohen's d effect size reported for the AUC-PR delta. Exact library versions are pinned in `requirements.txt`.

## Data Availability

Primary data consists of KNUST student survey responses and optional anonymized browser-interaction logs, collected only after CSEC ethics approval and individual informed consent. Raw participant data will **not** be committed to this repository, consistent with the anonymization and access-control plan in the proposal's ethics section. Aggregated, de-identified derived datasets may be released separately if approved by CSEC. The public MyPersonality benchmark (Kosinski et al., 2013) is used under its own academic-use terms and is not redistributed here.

## Citation

A Zenodo DOI will be registered for this repository (via GitHub–Zenodo integration) prior to the first tagged release, so that code is citable independently of the eventual journal publication. This README will be updated with the DOI badge once minted.

## License

To be determined prior to first code release.

## Contact

Dennis Agyapong Nkansah, Department of Computer Science, KNUST (Student ID: 20974738).
