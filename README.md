# t2d-prescribing-trajectories

Reproducible code, data-processing pipelines, clustering methods, and visualization tools used to characterize real-world glucose-lowering medication prescribing trajectories in adults with type 2 diabetes, using national EHR data from 2019–2024.

This repository accompanies the manuscript *"Medication Trajectories in Type 2 Diabetes in the United States, 2019–2024"* (Lux et al.). It contains the analysis pipeline from cohort construction through clustering, figures, and tables.

## Data availability

The analyses use de-identified national EHR data (TriNetX). Under the governing data use agreement, the underlying patient-level data **cannot be shared** and is not included in this repository. The notebooks expect locally available cleaned input tables (e.g., `medication_info.csv`, `lab_results.csv`, `BMI_vital_signs.csv`, `patient_demographics.csv`), which are produced by the dataset-construction step below. Notebooks were developed in .ipynb notebooks and read their inputs from a local data directory.

## Repository structure

The repository is organized into four stages that follow the analysis workflow.

### `Data Preprocessing/`

**Dataset Construction/** — builds the cleaned, analysis-ready tables from the raw EHR extracts:
- `CohortDatasetCreation.ipynb` — applies the inclusion/exclusion criteria and produces the cleaned cohort tables (demographics, medications, labs, vitals).
- `ComorbidityDatasetCreation.ipynb` — derives comorbidity indicators (e.g., heart failure, chronic kidney disease).
- `SLRDatasetCreation.ipynb` — assembles the dataset used for the regression analyses.

**Data Artifacts/** — derived representations used downstream:
- `MedicationTrajectoryRepresentations.ipynb` — encodes each patient's medication history into half-year interval representations (set-based `patient_bins` and one-hot `patient_vectors`) used for clustering.
- `get_closest_continuous_value_after_t0.ipynb` — extracts baseline HbA1c and BMI values (closest measurement before each patient's first prescription).

### `Analysis/`

- `ClusteringAnalysis.ipynb` — hierarchical agglomerative clustering of the trajectory vectors to identify prescribing-trajectory clusters, with cluster-quality evaluation.
- **Sensitivity Analysis/** — robustness checks supporting the main clustering result (alternative time binning, last-observation-carried-forward, sparsity filtering, prescription timing, and trajectory-eligibility comparisons). See the individual notebooks in this folder for the specifics of each.

### `Figure Creation/`

Notebooks that generate the manuscript and supplementary figures — cluster-level medication and metabolic-trajectory plots, patient-level heatmaps, and clustering-robustness figures — plus `PDFConversion.ipynb`, a utility for exporting figures.

### `Table Creation/`

Notebooks that generate the manuscript and supplementary tables, including the baseline characteristics table (`Table1_Construction.ipynb`), per-cluster breakdowns, the primary-vs-disengagement comparison, and the regression tables.

## General workflow

1. **Construct datasets** — run the `Dataset Construction/` notebooks to produce the cleaned cohort tables.
2. **Build derived representations** — generate the trajectory representations and baseline values in `Data Artifacts/`.
3. **Cluster** — run `ClusteringAnalysis.ipynb` to derive the prescribing-trajectory clusters.
4. **Generate outputs** — run the `Figure Creation/` and `Table Creation/` notebooks to reproduce the figures and tables.
5. **Sensitivity analyses** — run the notebooks in `Analysis/Sensitivity Analysis/` to reproduce the robustness checks.

Intermediate results are passed between notebooks as CSV and pickle files, so the stages above should be run in order.
