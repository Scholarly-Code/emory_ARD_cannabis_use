<div align="center">

# Language Models for Context-Aware Cannabis-Use Classification in Rheumatology Clinical Notes

**Emory Healthcare Cohort &nbsp;|&nbsp; NLP &nbsp;|&nbsp; LLM Benchmarking**

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?style=flat-square&logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-See%20LICENSE-green?style=flat-square)](LICENSE)
[![medRxiv](https://img.shields.io/badge/medRxiv-Wang%20et%20al.%202026-red?style=flat-square)](https://doi.org/10.64898/2026.03.06.26347824)
[![IRB](https://img.shields.io/badge/IRB-Approved-orange?style=flat-square)]()

<br>

*Validating the Stanford LLM benchmarking framework (Wang et al., 2026) for extracting*
*patient-reported cannabis use from EHR clinical notes,*
*applied to an independent Emory Healthcare cohort of autoimmune rheumatic disease (ARD) patients.*

</div>

---
## Table of Contents

- [Background](#background)
- [Cohort & ICD Codes](#cohort--icd-codes)
- [Project Structure](#project-structure)
- [Models & Pipeline](#models--pipeline)
- [Annotation Guidelines](#annotation-guidelines)
- [Getting Started](#getting-started)
- [Citation](#citation)
- [References](#references)
- [Acknowledgments](#acknowledgments)

---

## Background

Cannabis use is increasingly documented in clinical notes as patients with chronic pain conditions, including autoimmune rheumatic diseases (ARDs), report using cannabis for symptom management. Extracting this information at scale from unstructured EHR text enables:

- Characterizing trends in cannabis use among ARD patients
- Supporting observational research on pain management in rheumatology

This repository applies and validates the LLM-based NLP pipeline developed at Stanford (Wang et al., 2026) on an independent Emory Healthcare ARD cohort, using the same classification framework for **cannabis use status** (not mentioned / denial / past use / current use).

---

## Cohort & ICD Codes

Patients were identified from Emory Healthcare EHR data using validated ICD-10 case definitions for six autoimmune rheumatic diseases, requiring **2+ ICD codes with at least 1 code from a relevant specialist, 7 to 365 days apart**:

<div align="center">

| Disease | ICD-10 Codes | Specialist(s) |
|:---|:---:|:---|
| Ankylosing spondylitis | `M45.X` | Rheumatologist |
| Psoriatic arthritis | `L40.5X` | Rheumatologist, Dermatologist |
| Rheumatoid arthritis | `M05.X`, `M06.X` | Rheumatologist |
| Sjogren syndrome | `M35.0X` | Rheumatologist |
| Systemic lupus erythematosus | `M32.X` *(excl. M32.0)* | Rheumatologist, Dermatologist, Nephrologist |
| Systemic sclerosis | `M34.X` | Rheumatologist, Dermatologist |

</div>

> Case definitions follow the validated algorithm from Falasinnu et al. (2024).

EHR clinical notes were screened for cannabis-related mentions via [fuzzy and exact string matching](https://github.com/bozlab/pain-management-cannabis-ehr/blob/main/preprocess.py) against a curated keyword lexicon (similarity threshold >= 90), extracting 200-character context windows (+/- 100 characters) around each match.

---

## Project Structure

```
cannabis-ard-emory/
|
+-- 0_Cannabis_Emory_ARD_code.ipynb      # Main analysis notebook: cohort building,
|                                         # keyword screening, snippet extraction,
|                                         # and patient-level aggregation
|
+-- 1_medgemma_4b_it.py                  # MedGemma 4B inference script
|
+-- 2_llama_3_1_8b_it.py                 # LLaMA 3.1 8B inference script
|
+-- 3_gpt_oss_20b.py                     # GPT-OSS 20B inference script
|
+-- ANNOTATION_GUIDELINES__Emory_EHR_Cannabis.pdf
|                                         # Annotation guidelines for cannabis use
|                                         # status classification
|
+-- requirements.txt                     # Python dependencies (pip)
+-- environment.yml                      # Conda environment specification
+-- LICENSE
+-- README.md
```

---

## Models & Pipeline

<div align="center">

| Script | Model |
|:---|:---|
| `1_medgemma_4b_it.py` | MedGemma 4B Instruct |
| `2_llama_3_1_8b_it.py` | LLaMA 3.1 8B Instruct |
| `3_gpt_oss_20b.py` | GPT-OSS 20B |

</div>

The pipeline mirrors the Stanford benchmarking design (Wang et al., 2026):

```
EHR Clinical Notes
       |
       v
[1] Keyword Screening      -- fuzzy match against cannabis lexicon (threshold >= 90)
       |
       v
[2] Snippet Extraction     -- 200-character context windows (+/- 100 characters)
       |
       v
[3] LLM Classification     -- cannabis use status (4-class)
       |
       v
[4] Patient-Level Aggregation -- temporal trend analysis across the cohort
```

---

## Annotation Guidelines

The [`Annotation Guideline.pdf`](Annotation Guideline.pdf) describes the annotation schema used for this Emory cohort.

**Cannabis use status classes:**

| Label | Description |
|:---:|:---|
| 0 | Not a true cannabis mention / uncertain |
| 1 | Denial of use |
| 2 | Positive past use |
| 3 | Positive current use |

---

## Getting Started

### Option A: pip

```bash
pip install -r requirements.txt
```

### Option B: Conda

```bash
conda env create -f environment.yml
conda activate <env-name>
```

> **Note:** EHR data access requires appropriate IRB approval and institutional data use agreements. This repository contains only code and guidelines; no patient data are included.

### Running the pipeline

```bash
# Step 1: Cohort building and snippet extraction
jupyter notebook 0_Cannabis_Emory_ARD_code.ipynb

# Step 2: Run LLM classifiers
python 1_medgemma_4b_it.py
python 2_llama_3_1_8b_it.py
python 3_gpt_oss_20b.py
```

---

## Citation

If you use this code, please cite our work *(placeholder; to be updated upon publication)*:

```bibtex
@article{emory2026cannabis,
  title   = {TBD},
  author  = {TBD},
  journal = {TBD},
  year    = {2026},
  note    = {Under review}
}
```

Please also cite the original Stanford benchmarking study this work validates:

```bibtex
@article{wang2026cannabis,
  title   = {Extracting patient reported cannabis use and reasons for use from
             electronic health records: a benchmarking study of large language models},
  author  = {Wang, Yiyu and others},
  journal = {medRxiv},
  year    = {2026},
  doi     = {10.64898/2026.03.06.26347824},
  url     = {https://doi.org/10.64898/2026.03.06.26347824}
}
```

---

## References

1. Wang, Yiyu, et al. "Extracting patient reported cannabis use and reasons for use from electronic health records: a benchmarking study of large language models." *medRxiv* (2026): 2026-03. https://doi.org/10.64898/2026.03.06.26347824

2. Falasinnu T, et al. "Annual trends in pain management modalities in patients with newly diagnosed autoimmune rheumatic diseases in the USA from 2007 to 2021: an administrative claims-based study." *Lancet Rheumatology* (2024). https://doi.org/10.1016/S2665-9913(24)00120-6

3. Le, Nathan, et al. "Visualizing Real-World Pain Treatment Pathways in Chronic Disease: A Sequence-Based Analysis of Polypharmacy in Systemic Lupus Erythematosus." *ACR Open Rheumatology* 7.11 (2025): e70116. https://doi.org/10.1002/acr2.70116

4. Le, Nathan, et al. "REAL-WORLD ASSESSMENT OF PATTERNS OF PAIN TREATMENT IN SYSTEMIC LUPUS ERYTHEMATOSUS: A PATHWAY VISUALIZATION STUDY USING EHR." *The Journal of Rheumatology*. Vol. 52. No. Suppl 1. (2025). https://doi.org/10.3899/jrheum.2025-0390.PV254

5. Le, Nathan, et al. "Real-World Assessment of Patterns of Pain Treatment in Systemic Lupus Erythematosus: A Pathway Visualization Study Using Electronic Health Records." *The Journal of Pain* 29 (2025). https://doi.org/10.1016/j.jpain.2025.105129

---

## Acknowledgments

This work is a collaboration between **Emory University** and **Stanford University**. For inquiries, please open an issue or contact the repository maintainers.

<br>

<div align="center">
  <img src="figures/Emory.png" height="60" alt="Emory University" />
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img src="figures/Stanford.jpg" height="60" alt="Stanford University" />
</div>
