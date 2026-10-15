Yes Bubu. Click **Add a README**, then paste this directly into the README editor.

```markdown
# Leaf Density-Based Tomato Plant Growth Stage Classification

## Overview

This repository contains the implementation and experimental results of a deep learning framework developed for tomato plant growth stage classification using leaf-density-related structural information.

The proposed sequential pipeline is:

**Raw Tomato Plant Image → Segmentation → Leaf/Plant-Focused Image → ResNet Classifier → Growth Stage**

Two segmentation models were evaluated:

- U-Net
- DeepLabV3

Each segmentation model was combined with three ResNet classifiers:

- ResNet34
- ResNet50
- ResNet101

Therefore, six sequential pipelines were evaluated.

---

## Growth Stages

The dataset contains four tomato plant growth stages:

1. Developing
2. Flowering
3. Fruiting
4. Seeding

---

## Dataset

The dataset contains a total of **982 tomato plant images**.

| Split | Images |
|---|---:|
| Training | 687 |
| Validation | 135 |
| Test | 160 |
| **Total** | **982** |

For classifier cross-validation:

- Training + validation images were combined.
- Cross-validation pool = **822 images**
- External held-out test set = **160 images**

---

## Sequential Pipeline

```text
Raw Tomato Plant Image
          ↓
Segmentation Model
(U-Net / DeepLabV3)
          ↓
Predicted Binary Mask
       ↙          ↘
      ↓            ↓
LPF Calculation   Background Removal
      ↓            ↓
Structural       Masked Plant Image
Indicator             ↓
                  ResNet Classifier
                       ↓
                Growth Stage
```

---

## Leaf Pixel Fraction

Leaf Pixel Fraction is calculated as:

```text
LPF = Foreground Pixels / Total Image Pixels
```

LPF is used as a structural density-related indicator.

In this implementation:

- LPF is calculated separately from the segmentation mask.
- The same mask is used for background removal.
- The masked plant image is passed to the ResNet classifier.
- LPF is not directly provided as an input feature to ResNet.

---

## Evaluated Pipelines

### U-Net Based

- U-Net → ResNet34
- U-Net → ResNet50
- U-Net → ResNet101

### DeepLabV3 Based

- DeepLabV3 → ResNet34
- DeepLabV3 → ResNet50
- DeepLabV3 → ResNet101

---

## Cross-Validation Setup

A stratified five-fold classifier cross-validation strategy was used.

```text
Number of folds: 5
Shuffle: True
Random seed: 42
Cross-validation images: 822
External held-out test images: 160
```

For each fold:

1. A fresh pretrained ResNet classifier was initialized.
2. The classifier was trained using the fold training subset.
3. The best model was selected using validation accuracy.
4. The selected fold model was evaluated using the external 160-image test set.
5. Accuracy, macro precision, macro recall and macro F1-score were calculated.

---

## Training Configuration

| Parameter | Value |
|---|---|
| Input Size | 224 × 224 |
| Batch Size | 32 |
| Epochs | 20 |
| Optimizer | Adam |
| Learning Rate | 1e-4 |
| Scheduler | ReduceLROnPlateau |
| Loss Function | Cross-Entropy Loss |
| Number of Classes | 4 |
| Cross-Validation | Stratified 5-Fold |
| Random Seed | 42 |

---

## Overall Results

The following results are reported as **Mean ± Standard Deviation** across the five fold-trained models.

| Pipeline | Accuracy | Macro Precision | Macro Recall | Macro F1 |
|---|---:|---:|---:|---:|
| **U-Net → ResNet34** | **0.9150 ± 0.0130** | **0.9052 ± 0.0115** | **0.9105 ± 0.0286** | **0.9055 ± 0.0180** |
| U-Net → ResNet50 | 0.8875 ± 0.0303 | 0.8776 ± 0.0269 | 0.9022 ± 0.0125 | 0.8807 ± 0.0268 |
| U-Net → ResNet101 | 0.8963 ± 0.0302 | 0.8937 ± 0.0454 | 0.8763 ± 0.0464 | 0.8757 ± 0.0402 |
| DeepLabV3 → ResNet34 | 0.8988 ± 0.0093 | 0.9040 ± 0.0267 | 0.8617 ± 0.0188 | 0.8732 ± 0.0147 |
| DeepLabV3 → ResNet50 | 0.8463 ± 0.0363 | 0.8470 ± 0.0225 | 0.8735 ± 0.0038 | 0.8389 ± 0.0281 |
| DeepLabV3 → ResNet101 | 0.9000 ± 0.0198 | 0.8953 ± 0.0222 | 0.8745 ± 0.0420 | 0.8717 ± 0.0342 |

---

## Best Performing Pipeline

The best performing configuration is:

```text
Segmentation Model : U-Net
Classifier         : ResNet34
Accuracy           : 0.9150 ± 0.0130
Macro F1           : 0.9055 ± 0.0180
```

U-Net → ResNet34 achieved the highest overall mean classification accuracy among the six evaluated pipelines.

---

---

## Main Libraries

The experiments were implemented using:

- Python
- PyTorch
- Torchvision
- NumPy
- Pandas
- Scikit-learn
- Matplotlib
- Pillow
- tqdm

GPU acceleration was used during model training and evaluation.

---

## Research Contribution

This work investigates a tomato-specific whole-plant growth stage classification framework combining:

- Plant segmentation
- Background removal
- Leaf-density-related structural analysis
- Leaf Pixel Fraction
- ResNet-based classification
- Five-fold cross-validation
- External held-out test evaluation

The approach focuses on whole tomato plant phenological stage classification rather than only tomato fruit detection or maturity recognition.

---

## Authors

Undergraduate Research Project  
Faculty of Engineering  
University of Jaffna, Sri Lanka

---


