# ForensicGuard

Instance-level forensic analysis for physical adversarial patch detection.

[![Python 3.10+](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0%2B-red)](https://pytorch.org/)

## Overview

**ForensicGuard** is a framework that leverages image forensic features and instance-level semantic analysis for detecting physical adversarial patches. 

**Key insight**: A physical patch attached to an object surface introduces forensic inconsistencies (SPN noise, DCT frequency anomalies, CLIP semantic deviations) that can be detected through cross-instance analysis.

**Key results**:
- Physical patch detection: **91.0%** accuracy
- Classic tamper detection: **97.0%** (augmented+COCO), **93.2%** (Columbia)
- Zero digital-to-physical domain gap via PC simulation

## Architecture

```
Input Image → YOLO26-seg Instance Segmentation 
           → Per-Instance SPN/DCT/CLIP Feature Extraction
           → Cross-Instance Consistency Analysis
           → MLP Fusion Classifier → Tamper/Patch Detection
```

## Repository Structure

```
code/
├── dataset/           # Dataset loaders (CASIA v2, Columbia, COCO)
├── features/          # Feature extractors (SPN, DCT, CLIP, segmenter)
├── models/            # MLP fusion classifier + consistency analysis
├── scripts/           # Training, evaluation, patch generation, PC simulation
├── plotting/          # Figure generation scripts
├── run_all.sh         # One-click experiment pipeline (10 stages)
└── download_datasets.sh  # Dataset download guide
```

## Requirements

- Python 3.10+
- PyTorch 2.0+
- torchvision, transformers, ultralytics, scikit-learn, scikit-image

## Quick Start

```bash
# 1. Install dependencies
pip install torch torchvision transformers ultralytics scikit-learn scikit-image

# 2. Run full experiment pipeline
bash code/run_all.sh

# 3. View results
bash code/run_all.sh --status
cat results/all_results.txt
```

## Pipeline Stages

| Stage | Description | Est. Time |
|:------|:------------|:----------|
| 1 | Generate v2 patch dataset (128×128 patches + PC simulation) | 15 min |
| 2 | Feature extraction for patch dataset | 120 min |
| 3 | Digital domain baseline training | 8 min |
| 4 | Physical domain evaluation | 3 min |
| 5 | Enhanced training (with physical samples) | 8 min |
| 6 | Classic tamper detection (CASIA/Columbia/COCO) | 45 min |
| 7 | Ablation experiments (4 configurations) | 30 min |
| 8 | Results summary | 2 min |
| 9 | SOTA baseline comparison (ResNet-18 + global features) | 20 min |
| 10 | CASIA v2 feature re-extraction + FocalLoss training | 60 min |

## Citation

```bibtex
@inproceedings{ouyang2027forensicguard,
  title={ForensicGuard: Instance-Level Forensic Analysis for Physical Patch Detection},
  author={Ouyang, Pan and Ouyang, Junlin},
  booktitle={MMM},
  year={2027}
}
```
