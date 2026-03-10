# MAT_SCI 465: Week 03 & 04 Combined Assignment
## Classical, ML, and Deep Learning Approaches to Microscopy Analysis

### Dataset
DOPAD (Dataset Of nanoPArticle Detection) - 100 TEM images (416x416 crops) were used for the classical pipeline.
Task 3 used 30 LabelMe pixel-level masks for supervised deep learning experiments.
Source: https://dopad.github.io/

### Objective
Compare classical computer vision, machine learning, and deep learning approaches on the same microscopy dataset, then summarize their performance, runtime, labeling needs, and interpretability.

### Task 1: Classical Image Analysis Pipeline
Method:
- Median filtering with disk radius 3 for noise reduction
- CLAHE with clip limit 0.025 for local contrast enhancement
- Otsu thresholding followed by Watershed segmentation
- Morphology and intensity measurements extracted with `skimage.measure.regionprops`

Results:
- 9247 particles detected across 100 images
- Mean SNR improved from 5.74 to 6.28 after filtering
- Runtime: 0.32 s/image

Output files:
- `classical_results.csv`
- `task1_four_panel.png`

### Task 2: Machine Learning Approaches
Feature engineering:
- 16 descriptors per region
- Morphology: area, perimeter, equivalent diameter, eccentricity, solidity, circularity, extent
- Intensity: mean intensity, standard deviation of intensity
- Edge: Canny edge ratio, Sobel mean, Sobel standard deviation
- Blob/LoG: blob count, blob mean sigma
- Texture/LBP: LBP mean, LBP standard deviation

Labeling:
- Region labels were initialized automatically and then manually reviewed and corrected before supervised training.
- 4292 labeled regions were matched to extracted features.
- Balanced training subset: 200 samples per class (400 total).

Model results:
- SVM: F1 = 0.810, Precision = 0.829, Recall = 0.812, Runtime = 0.003 s
- Random Forest: F1 = 0.874, Precision = 0.891, Recall = 0.875, Runtime = 0.199 s
- K-Means: best k = 3, Silhouette = 0.199

Comparison with classical segmentation:
- Classical Watershed detected 9247 particles across 100 images.
- ML analysis covered 4292 labeled regions across 50 images.
- Random Forest classified 3476 of 4292 regions (81.0%) as large/elongated.

Output files:
- `ml_results.csv`
- `task2_feature_importance.png`
- `task2_confusion_matrices.png`
- `task2_clustering.png`

### Task 3: Deep Learning and Final Comparison
Data preparation:
- 30 LabelMe masks loaded from `labelme_mask_30`
- Leak-free split by original image first: 24 training images and 6 validation images
- Deterministic augmentation applied to training only: horizontal flip, vertical flip, 90 degree rotation, Gaussian noise, and intensity shift
- Training set after augmentation: 144 images
- Validation set kept unaugmented to avoid data leakage

CNN classification:
- Compact CNN with 3 convolution blocks (32, 64, 128 filters), batch normalization, pooling, dropout, and dense head
- Trained on particle patches extracted separately from training and validation images
- Validation F1 = 0.333
- Runtime = 9.2 s

U-Net segmentation:
- Encoder-decoder U-Net with skip connections
- Weighted Dice + BCE loss for foreground/background imbalance
- Validation metrics on 6 held-out original images: IoU = 0.758, Dice = 0.859
- Runtime = 633.1 s

Output files:
- `task3_cnn_learning_curves.png`
- `task3_unet_learning_curves.png`
- `task3_unet_segmentation.png`
- `task3_feature_maps.png`

### Method Comparison
| Method | Task | Key Metric | Runtime | Labels Needed | Interpretability |
| --- | --- | --- | --- | --- | --- |
| Watershed | Segmentation | 9247 particles | 0.32 s/image | None | High |
| SVM | Classification | F1 = 0.810 | 0.003 s | 50+ labeled regions | Medium |
| Random Forest | Classification | F1 = 0.874 | 0.199 s | 50+ labeled regions | Medium |
| K-Means | Clustering | Silhouette = 0.199 | Fast (<1 s) | None | Low |
| CNN | Classification | F1 = 0.333 | 9.2 s | Particle patches + labels | Low |
| U-Net | Segmentation | IoU = 0.758, Dice = 0.859 | 633.1 s | 30 pixel-level masks | Low |

### Recommended Use Cases
- Quick exploratory analysis: classical Watershed pipeline
- Region-level classification with interpretable features: Random Forest
- Unsupervised structure discovery: K-Means with PCA/t-SNE
- Pixel-level segmentation: U-Net

### Deliverables Included
- `classical_results.csv`
- `ml_results.csv`
- `comparison_table.csv`
- `particle_labels.csv`
- `task1_four_panel.png`
- `task2_feature_importance.png`
- `task2_confusion_matrices.png`
- `task2_clustering.png`
- `task3_cnn_learning_curves.png`
- `task3_unet_learning_curves.png`
- `task3_unet_segmentation.png`
- `task3_feature_maps.png`
- `final_3x3_panel.png`
- `README.md`

### Notes
All core assignment deliverables are present in the workspace. The only item outside this folder is the final submission step on Canvas, including the GitHub repository URL and any final publication-quality formatting decisions required by the course.
