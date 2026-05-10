# Multimodal EO-SAR Change Detection

## Overview
This project performs binary change detection using paired EO (Electro Optical) and SAR (Synthetic Aperture Radar) satellite imagery.

The objective is to detect disaster-related building changes between pre-event and post-event imagery.

---

## Dataset

Dataset structure:

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

---

## Label Remapping

Original labels were remapped as:

| Original | New |
|---|---|
| 0 | 0 |
| 1 | 0 |
| 2 | 1 |
| 3 | 1 |

Where:
- 0 = No Change
- 1 = Change

---

## Model

- U-Net
- ResNet34 Encoder
- Binary Segmentation

---

## Loss Function

Combined:
- BCEWithLogitsLoss
- Dice Loss

Class imbalance handled using:
- pos_weight = 8.0

---

## Training

Run training:

```python
python train.py
```

---

## Evaluation Metrics

- IoU
- Precision
- Recall
- F1 Score

---

## Results

| Metric | Score |
|---|---|
| Validation IoU | 0.1427 |
| Precision | 0.0177 |
| Recall | 0.4140 |
| F1 Score | 0.0340 |

---

## Requirements

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Model Weights

best_model.pth

---

## References

- U-Net
- Segmentation Models PyTorch
- Albumentations
