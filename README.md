# Polygenic Liability Moderates Treatment Response for Smoking Cessation: A Study in the All of Us Research Program

Analysis code for:

> Luo Z, Mathews HL, Sarabudla N, Huang F, Bogdan R, Agrawal A, Chen L-S, Johnson EC, Hatoum AS.
> *Polygenic Liability Moderates Treatment Response for Smoking Cessation: A Study in the All of Us Research Program.* (in submission)
> <!-- TODO: add journal, year and DOI once published -->

## Data access

All data come from the [All of Us Research Program](https://www.researchallofus.org/) **Controlled Tier, CDR v8**.
Under the All of Us Data and Statistics Dissemination Policy, participant-level data cannot be shared, so this
repository contains **code only**. To reproduce the analyses you need registered Controlled Tier access.
Then clone this repository into a Researcher Workbench workspace (R kernel) and run the notebooks in order.

## Notebooks (in the order the paper presents them)

| Notebook | Paper section | Main output |
|---|---|---|
| `01_cohort_smokers.ipynb` | Methods: Study Population (Tables S1–S2) | `smokers_cohort.csv` |
| `02_treatment.ipynb` | Methods: Cessation Pharmacotherapy Receipt (Table S3) | `cohort_treatment.csv` |
| `03_covariates_outcome.ipynb` | Methods: Polygenic Score, Covariates, SDoH, Self-reported Smoking Behaviors | `person_covariates.csv`, `DF_smokerV2.csv`, `DF_smokerV3.csv` |
| `04_descriptives.ipynb` | Results: Sample characteristics (Table 1) | — |
| `05_aim1_pgs_cessation.ipynb` | Aim 1: PGS → cessation (Figure 1; Tables S11–S14) | `aim1_pgs_cessation_results.csv` |
| `06_aim2_moderation.ipynb` | Aim 2: PGS × treatment moderation, psychiatric adjustment, IPW (Figure 2; Tables S16–S21) | `interaction_estimates.csv`, `psych_model_tables.xlsx`, `ipw_*.csv/xlsx` |
| `07_aim3_varenicline_mediation.ipynb` | Aim 3: varenicline recipients, SVE/LVE, mediation by CPD (Figure 3; Table S22) | `varenicline_effectiveness_results.xlsx`, `aim3_mediation_*.csv` |
| `supplement_treatment_receipt.ipynb` | Supplementary Results: do PGS predict who receives treatment? | `supp_treatment_receipt_results.csv` |

`R/helpers.R` contains the shared functions: workspace-bucket I/O, factor coding, descriptive tables and model extraction.
Every notebook sources it, so run the notebooks from the repository root.

Intermediate files are written to and read from `$WORKSPACE_BUCKET/data/`, so no workspace-specific paths are hard-coded.

## Inputs created outside these notebooks

These files must already be in `$WORKSPACE_BUCKET/data/` before running notebook 03:

| File | How it was made |
|---|---|
| `prs_cig_per_day.csv`, `prs_cig_afrV2.csv`, `prs_ces_eur.csv`, `prs_ces_afr.csv` | Ancestry-specific PGS: PRS-CSx weights from GSCAN (Saunders et al., 2022) CPD and cessation summary statistics with 1000 Genomes EUR/AFR LD panels, scored in PLINK (PLINK `.sscore`-style output: FID, IID, PHENO, CNT, CNT2, SCORE) |
| `eur_pca.eigenvec`, `ancestry_specific_pcs/afr_pca.eigenvec` | 10 within-ancestry genetic PCs (see Supplemental Methods for ancestry assignment) |
| `psychiatric_diagnoses_EHR_V2.csv` | EHR-derived indicators (0/1) for SCZ, MDD, ADHD, bipolar disorder, anxiety and PTSD |

<!-- TODO: add the PRS-CSx / PLINK / PCA scripts and the psychiatric-phenotype script under scripts/, or link to where they are published. -->

## Software

R 4.5.0 on the All of Us Researcher Workbench. Key packages: `tidyverse`, `bigrquery`, `allofus`, `WeightIt` (2.1.0), `cobalt`,
`survey`, `lavaan`, `ggeffects`, `broom`, `writexl`, `openxlsx`, `ggdist`, `ggsignif`. Each notebook installs any it is missing.

## Notes

- Notebooks are committed **without outputs**. Before committing re-run outputs, make sure nothing prints participant-level data
  or counts below 20, as the All of Us dissemination policy requires.
- Cells marked `NOTE (cleanup)` show where the analysis code was changed during cleanup (bug fixes, explicit joins)
  or where a choice in the code should be checked against the manuscript.

## Contact

Zhen Luo, Department of Psychiatry, Washington University School of Medicine. <!-- TODO: email -->
