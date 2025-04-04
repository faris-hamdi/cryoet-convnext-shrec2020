# Assessing ConvNeXt V1 and V2 for Protein Classification on the SHREC 2020 Cryo-Electron Tomography Dataset

This repository presents the complete implementation and analysis of **ConvNeXt V1** and **ConvNeXt V2** models, pretrained on ImageNet, for classifying macromolecular complexes in the SHREC 2020 Cryo-Electron Tomography (Cryo-ET) dataset.

---

## Overview

Cryo-electron tomography (Cryo-ET) enables high-resolution, three-dimensional imaging of macromolecular structures in near-native cellular environments. However, its low signal-to-noise ratio and inherent imaging constraints present significant challenges for downstream analysis—particularly protein particle classification.

This project applies **ConvNeXt V1 and V2**—modern convolutional neural networks pretrained on ImageNet—to classify protein particles extracted from simulated cryo-ET volumes.

- **Dataset**: SHREC 2020 Cryo-ET benchmark (10 tomograms with ground-truth labels)
- **Models**: ConvNeXt V1 and V2, fine-tuned for 12-class classification
- **Representation**: Three orthogonal slices (XY, XZ, YZ) extracted from \(48^3\) voxel subtomograms
- **Pipeline**: Fully implemented in a single Jupyter notebook

---

## Project Structure

```text
cryoet-convnext-shrec2020/
│
├── notebook/                  # Full pipeline: preprocessing, training, inference
│   └── convnext_shrec2020_classification.ipynb
│
├── figures/                   # Class distributions, loss/accuracy plots, confusion matrices, F1 plots
│
├── report/                    # Final project report
│   └── SHREC2020_ConvNeXt_Report.pdf
│
├── requirements.txt           # Python dependencies
├── LICENSE                    # MIT License
├── .gitignore                 # Ignore cache, large files, and checkpoints
└── README.md                  # This file
```

---

## Dataset Access

Due to GitHub file size constraints, both the original and processed SHREC 2020 datasets are hosted externally.

### External Access

- **Official Source**: [SHREC 2020 Dataset (Dataverse)](https://dataverse.nl/dataset.xhtml?persistentId=doi:10.34894/Y2ZMRH)
- **Google Drive Mirror**: [Dataset Folder](https://drive.google.com/drive/folders/1ROgxmjFOAZoFKB19cl94RgeFqtbcCfjz?usp=sharing)

### Google Drive Directory Contents

| Folder/File              | Description                                                                |
|--------------------------|----------------------------------------------------------------------------|
| `shrec2020_full_dataset/` | Raw tomograms 0–9 with class metadata                                      |
| `slice_dataset/`          | Extracted XY/XZ/YZ slices from tomograms 0–8                               |
| `slice_triplets.csv`      | Mapping from slice triplets to protein class labels                        |
| `processed_triplets.zip`  | Processed data: `.pt` files (triplet views + class labels)                |
| `saved_models/`           | Pretrained ConvNeXt V1/V2 models and checkpoints                           |

> **Note**: Tomograms 0–8 are used for training/validation (80/20 split), while tomogram 9 is held out for testing.

---

## Pretrained Models

Pretrained model weights and full training checkpoints are available in the `saved_models/` directory (see Google Drive).

### Included Files

| File                                   | Description                                        |
|----------------------------------------|----------------------------------------------------|
| `best_convnextv1_hf.pth`               | Final ConvNeXt V1 model (best at epoch 20)         |
| `best_convnextv1_hf.pth_checkpoint.pt` | Checkpoint with optimizer/scheduler state          |
| `best_convnextv2_hf.pth`               | Best ConvNeXt V2 model (early-stopped at epoch 14) |
| `best_convnextv2_hf.pth_checkpoint.pt` | Full training checkpoint for ConvNeXt V2           |

> Training used **mixed precision**, **class-weighted cross-entropy**, **early stopping**, and a **gradual unfreezing** strategy (stage 3 unfrozen after epoch 5).

---

## Results

The models were evaluated on **tomogram 9**, which contains 2782 protein particles across 12 classes. Both ConvNeXt models showed excellent performance:

### Overall Metrics

| Model        | Accuracy | Precision | Recall  | F1 Score |
|--------------|----------|-----------|---------|----------|
| ConvNeXt V1  | 0.9894   | 0.9887    | 0.9887  | 0.9887   |
| ConvNeXt V2  | 0.9868   | 0.9868    | 0.9856  | 0.9857   |

---

### Class-wise F1 Score Comparison

> Below is a comparison with leading SHREC 2020 submissions. **Bold** indicates the best result per class.

| Method        | 1s3x | 3qm1 | 3gl1 | 3h84 | 2cg9 | 3d2f | 1u6g | 3cf3 | 1bxn | 1qvr | 4cr2 | 4d8q |
|---------------|------|------|------|------|------|------|------|------|------|------|-------|-------|
| 3D MS-D       | 0.192 | 0.408 | 0.437 | 0.416 | 0.368 | 0.461 | 0.492 | 0.719 | 0.948 | 0.851 | 0.942 | 0.964 |
| DeepFinder    | 0.610 | 0.729 | 0.800 | 0.911 | 0.783 | 0.848 | 0.866 | 0.939 | **1.000** | 0.984 | 0.993 | 0.993 |
| YOPO          | 0.558 | 0.741 | 0.670 | 0.834 | 0.696 | 0.682 | 0.795 | 0.896 | 0.987 | 0.830 | 0.923 | 0.993 |
| UMC           | 0.661 | 0.827 | 0.839 | 0.947 | 0.855 | 0.873 | 0.899 | 0.981 | 0.997 | 0.980 | **1.000** | 0.997 |
| ConvNeXt V1   | **0.967** | **0.966** | 0.989 | **0.997** | **0.986** | 0.982 | **0.986** | 0.995 | **1.000** | **0.995** | **1.000** | **1.000** |
| ConvNeXt V2   | 0.940 | 0.943 | **0.994** | **0.997** | 0.983 | **0.994** | 0.983 | **1.000** | **1.000** | **0.995** | 0.998 | **1.000** |

> Full evaluation results, training logs, and visualizations are included in the [report](report/SHREC2020_ConvNeXt_Report.pdf) and the `figures/` directory.

---

## License

This project is licensed under the [MIT License](LICENSE).

