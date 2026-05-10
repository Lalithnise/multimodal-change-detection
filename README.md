# Multimodal EO-SAR Change Detection

## Overview

This project performs binary change detection using paired EO (Electro Optical) and SAR (Synthetic Aperture Radar) satellite imagery.

The objective is to detect disaster-related building changes between pre-event and post-event imagery using deep learning-based semantic segmentation.

The project was implemented as part of a multimodal disaster assessment assignment.

---

## Dataset

Dataset source:

[Hugging Face Change Detection Dataset](https://huggingface.co/datasets/doron333/change-detection-dataset)

The dataset is not included in this repository due to large file size constraints.

### Dataset Structure

```text
train/
│
├── pre-event/
├── post-event/
└── target/

val/
│
├── pre-event/
├── post-event/
└── target/

test/
│
├── pre-event/
├── post-event/
└── target/
```

### Dataset Setup

Download:
- train.zip
- val.zip
- test.zip

Then unzip using:

```python
!unzip train.zip -d train
!unzip val.zip -d val
!unzip test.zip -d test
```

---

## Label Remapping

Original labels were remapped into binary classes:

| Original Label | Meaning | New Label |
|---|---|---|
| 0 | Background | 0 |
| 1 | Intact | 0 |
| 2 | Damaged | 1 |
| 3 | Destroyed | 1 |

Where:
- 0 = No Change
- 1 = Change

---

## Model Architecture

The segmentation model uses:

- U-Net
- ResNet34 encoder
- Binary segmentation output

Input:
- 6-channel tensor
  - RGB pre-event image
  - RGB post-event image

---

## Preprocessing

- Images resized from 1024×1024 to 256×256
- EO and SAR images concatenated channel-wise
- Data augmentation performed using Albumentations

Augmentations used:
- Horizontal Flip
- Vertical Flip
- Random Rotation
- Brightness/Contrast Augmentation

---

## Loss Function

Combined loss:
- BCEWithLogitsLoss
- Dice Loss

To address severe class imbalance:

```python
pos_weight = 8.0
```

was used during weighted BCE training.

---

## Training Configuration

| Parameter | Value |
|---|---|
| Image Size | 256×256 |
| Batch Size | 4 |
| Epochs | 5 |
| Optimizer | Adam |
| Learning Rate | 1e-4 |
| GPU | NVIDIA Tesla T4 |

---

## Evaluation Metrics

The following metrics were used:

- IoU (Intersection over Union)
- Precision
- Recall
- F1 Score

---

## Results

### Experiment 1 — BCE + Dice Loss

The initial model suffered from severe background collapse due to class imbalance.

| Metric | Score |
|---|---|
| Precision | 0.0000 |
| Recall | 0.0000 |
| F1 Score | 0.0000 |

---

### Experiment 2 — Weighted BCE + Dice Loss

Weighted BCE improved foreground sensitivity and recall.

| Metric | Score |
|---|---|
| Validation IoU | 0.1427 |
| Precision | 0.0177 |
| Recall | 0.4140 |
| F1 Score | 0.0340 |

---

## Key Challenges

- Severe class imbalance
- Sparse foreground regions
- EO↔SAR modality differences
- SAR speckle noise
- False positive predictions after weighted training

---

## Future Improvements

Potential improvements include:

- Focal Loss
- Tversky Loss
- Attention UNet
- Transformer-based multimodal fusion
- Better class-balanced sampling
- Multi-scale training
- Threshold optimization

---

## Installation

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Running the Notebook

Open the notebook in Google Colab:

```text
multimodal_change_detection.ipynb
```

Upload:
- train.zip
- val.zip
- test.zip

Then run all cells sequentially.

---

## Model Weights

The trained checkpoint:

```text
best_model.pth
```

is included in this repository.

---

## Technologies Used

- Python
- PyTorch
- Segmentation Models PyTorch
- Albumentations
- OpenCV
- NumPy
- Matplotlib
- Google Colab

---

## References

- U-Net
- Segmentation Models PyTorch
- Albumentations
- Hugging Face Datasets
- EO/SAR Remote Sensing Literature
