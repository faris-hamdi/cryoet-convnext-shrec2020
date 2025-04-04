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

Both ConvNeXt models achieved high classification performance:

- **F1 scores above 98% overall**
- **Strong per-class consistency**
- **Competitive with state-of-the-art methods in [SHREC 2020](https://doi.org/10.1016/j.cag.2020.07.010)**

Full metrics, class-wise comparisons, and visualizations are available in the [report](report/SHREC2020_ConvNeXt_Report.pdf) and `figures/` directory.

---
