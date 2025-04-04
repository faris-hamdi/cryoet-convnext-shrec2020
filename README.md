# Assessing ConvNeXt V1 and V2 for Protein Classification on the SHREC 2020 Cryo-Electron Tomography Dataset

This repository presents the implementation and analysis of pretrained ConvNeXt V1 and ConvNeXt V2 models for classifying macromolecular complexes in the SHREC 2020 Cryo-Electron Tomography (Cryo-ET) dataset.

---

## Overview

Cryo-electron tomography (cryo-ET) enables three-dimensional visualization of macromolecular structures in their near-native environments. However, its inherently low signal-to-noise ratio and imaging constraints demand robust computational methods for downstream particle classification.

This project applies ConvNeXt V1 and V2—modern convolutional neural network (ConvNet) architectures pretrained on ImageNet—to classify protein particles extracted from simulated cryo-ET volumes.

- **Dataset**: SHREC 2020 Cryo-ET benchmark (10 tomograms with ground truth)
- **Models**: ConvNeXt V1 and V2, fine-tuned for 12-class classification
- **Representation**: Three orthogonal slices (XY, XZ, YZ) extracted from $48^3$ voxel subtomograms
- **Pipeline**: All steps implemented in a single Jupyter notebook

---

## Project Structure

```text
cryoet-convnext-shrec2020/
│
├── notebook/                  # End-to-end pipeline: preprocessing, training, inference
│   └── convnext_shrec2020_classification.ipynb
│
├── figures/                   # Processed samples, Class Distribution, Loss & accuracy curves, confusion matrices, F1 plots
│
├── report/                    # Final project report
│   └── SHREC2020_ConvNeXt_Report.pdf
│
├── requirements.txt           # Python dependencies
├── LICENSE                    # MIT License
├── .gitignore                 # Ignore cache, checkpoints, and large files
└── README.md                  # You are here
```

---

## Dataset Access

Due to GitHub's file size limitations, the SHREC 2020 dataset and derived files are hosted externally.

### Access Links

- **Official Dataset**: [SHREC 2020 on Dataverse](https://dataverse.nl/dataset.xhtml?persistentId=doi:10.34894/Y2ZMRH)
- **Google Drive**: [SHREC2020 Dataset Folder](https://drive.google.com/drive/folders/1ROgxmjFOAZoFKB19cl94RgeFqtbcCfjz?usp=sharing)

### Folder Contents

| Folder/File              | Description                                                                |
|--------------------------|----------------------------------------------------------------------------|
| `shrec2020_full_dataset/` | Raw tomograms 0–9 with metadata                                            |
| `slice_dataset/`          | XY/XZ/YZ views for particles in tomograms 0–8                             |
| `slice_triplets.csv`      | Class labels for each triplet slice                                       |
| `processed_triplets.zip`  | `.pt` files for training/validation inputs (triplets + class)             |
| `saved_models/`           | Pretrained model weights and training checkpoints                         |


> **Note**: Tomograms 0–8 are used for training and validation (80/20 split); tomogram 9 is reserved for testing.

---

## Pretrained Models

Pretrained ConvNeXt model weights and checkpoints are available via Google Drive (see `saved_models/`).

### Files

| File                              | Description |
|-----------------------------------|-------------|
| `best_convnextv1_hf.pth`          | Best ConvNeXt V1 model (epoch 20) |
| `best_convnextv1_hf.pth_checkpoint.pt` | Full checkpoint with optimizer/scheduler state |
| `best_convnextv2_hf.pth`          | Best ConvNeXt V2 model (early stopped at epoch 14) |
| `best_convnextv2_hf.pth_checkpoint.pt` | Full checkpoint for ConvNeXt V2 |


> Training used mixed precision, class weighting, early stopping, and a gradual unfreezing strategy (stage 3 unfrozen after epoch 5).

---

## Results

Certainly! Here’s a **concise and coherent version** of the results section with both tables clearly presented, trimmed of repetition while retaining all critical details:

---

### 📈 Results

The models were evaluated on the test tomogram (Tomogram 9), containing 2782 particles from 12 protein classes. Both ConvNeXt V1 and V2 demonstrated high performance:

#### Overall Performance

| Model        | Accuracy | Precision | Recall  | F1 Score |
|--------------|----------|-----------|---------|----------|
| ConvNeXt V1  | 0.9894   | 0.9887    | 0.9887  | 0.9887   |
| ConvNeXt V2  | 0.9868   | 0.9868    | 0.9856  | 0.9857   |

---

#### F1 Score Comparison per class with leading methods from [the SHREC 2020 challenge](https://doi.org/10.1016/j.cag.2020.07.010)

> **Bold** values indicate the highest score per class.

| Method        | 1s3x | 3qm1 | 3gl1 | 3h84 | 2cg9 | 3d2f | 1u6g | 3cf3 | 1bxn | 1qvr | 4cr2 | 4d8q |
|---------------|------|------|------|------|------|------|------|------|------|------|-------|-------|
| 3D MS-D       | 0.192 | 0.408 | 0.437 | 0.416 | 0.368 | 0.461 | 0.492 | 0.719 | 0.948 | 0.851 | 0.942 | 0.964 |
| DeepFinder    | 0.610 | 0.729 | 0.800 | 0.911 | 0.783 | 0.848 | 0.866 | 0.939 | **1.000** | 0.984 | 0.993 | 0.993 |
| 3D ResNet     | 0.193 | 0.185 | 0.405 | 0.407 | 0.334 | 0.445 | 0.491 | 0.628 | 0.906 | 0.719 | 0.868 | 0.817 |
| YOPO          | 0.558 | 0.741 | 0.670 | 0.834 | 0.696 | 0.682 | 0.795 | 0.896 | 0.987 | 0.830 | 0.923 | 0.993 |
| Dn3DUnet      | 0.529 | 0.577 | 0.569 | 0.674 | 0.332 | 0.523 | 0.462 | 0.676 | 0.925 | 0.684 | 0.907 | 0.974 |
| UMC           | 0.661 | 0.827 | 0.839 | 0.947 | 0.855 | 0.873 | 0.899 | 0.981 | 0.997 | 0.980 | **1.000** | 0.997 |
| TM-T          | 0.200 | 0.102 | 0.248 | 0.727 | 0.555 | 0.869 | 0.835 | 0.880 | 0.934 | 0.970 | 0.968 | 0.945 |
| TM-F          | 0.319 | 0.219 | 0.207 | 0.660 | 0.589 | 0.808 | 0.815 | 0.945 | 0.939 | 0.966 | 0.968 | 0.945 |
| ConvNeXt V1   | **0.967** | **0.966** | 0.989 | **0.997** | **0.986** | 0.982 | **0.986** | 0.995 | **1.000** | **0.995** | **1.000** | **1.000** |
| ConvNeXt V2   | 0.940 | 0.943 | **0.994** | **0.997** | 0.983 | **0.994** | 0.983 | **1.000** | **1.000** | **0.995** | 0.998 | **1.000** |


> Full metrics, class-wise comparisons, and visualizations are available in the [report](report/SHREC2020_ConvNeXt_Report.pdf) and `figures/` directory.

---
