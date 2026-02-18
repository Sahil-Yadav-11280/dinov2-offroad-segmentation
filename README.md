# Off-Road Semantic Segmentation using DINOv2

## Project Overview

This project implements a semantic segmentation pipeline for off-road environments using a pretrained DINOv2 Vision Transformer backbone combined with a lightweight ConvNeXt-style segmentation head.

The objective is to accurately segment terrain components such as trees, bushes, rocks, logs, landscape, sky, and ground clutter to support off-road autonomous navigation scenarios.

---

## Model Architecture

### Backbone
- **DINOv2 (ViT-S/14 pretrained)**
- Used as a frozen feature extractor
- Extracts patch-level visual embeddings

### Segmentation Head
- ConvNeXt-style convolutional decoder
- Maps transformer embeddings to pixel-wise class predictions
- Outputs 10 semantic classes

---

## Classes

The model predicts the following 10 classes:

1. Background  
2. Trees  
3. Lush Bushes  
4. Dry Grass  
5. Dry Bushes  
6. Ground Clutter  
7. Logs  
8. Rocks  
9. Landscape  
10. Sky  

---

## Dataset Structure

The expected dataset format:

Offroad_Segmentation_Training_Dataset/

├── train/

│ ├── Color_Images/

│ └── Segmentation/

└── val/

├── Color_Images/

└── Segmentation/


Each image in `Color_Images` must have a corresponding segmentation mask with the same filename in `Segmentation`.

---

## Training Configuration

- Input Resolution: 480 × 960 (aligned to patch size multiple of 14)
- Batch Size: 2
- Optimizer: SGD
- Loss Function: Cross-Entropy Loss
- Epochs: 10 (with optional fine-tuning)
- Evaluation Metrics:
  - Mean Intersection over Union (mIoU)
  - Dice Score
  - Pixel Accuracy

---

## Final Validation Results

- **Mean IoU:** 0.2918  
- **Dice Score:** 0.4349  
- **Pixel Accuracy:** 0.7019  

The model showed consistent performance improvement across epochs without overfitting.

---

## How to Train

<code>python train_segmentation.py</code>

This will:

Train the segmentation head

Save model weights (segmentation_head.pth)

Generate training curves

Save evaluation metrics

## How to Evaluate

<code>python test_segmentation.py</code>

Outputs will be saved in:

predictions/

├── masks/

├── masks_color/

├── comparisons/

├── evaluation_metrics.txt

└── per_class_metrics.png

## Observations

1. Validation IoU increased steadily across epochs.

2. No signs of overfitting were observed.

3. Larger terrain classes (background, sky) achieved higher accuracy.

4. Smaller object classes can benefit from class-weighted loss and extended training.

## Future Improvements

1. Class-weighted cross-entropy to handle imbalance

2. Hybrid Dice + Cross-Entropy loss

3. Longer training schedule

4. Data augmentation techniques

5. Fine-tuning the backbone# dinov2-offroad-segmentation
