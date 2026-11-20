# BCCD Object Detection Using YOLO11n

## 1. Project Overview

This project focuses on object detection in microscopic blood-cell images using deep learning. A pretrained YOLO11n object detection model was fine-tuned on the BCCD (Blood Cell Count and Detection) dataset.

The objective is to detect and classify three types of blood cells:

- RBC — Red Blood Cells
- WBC — White Blood Cells
- Platelets

The project includes dataset preparation, transfer learning, a controlled experiment, final model evaluation, and failure analysis.

## 2. Dataset

**Dataset:** BCCD — Blood Cell Count and Detection

**Source:** https://github.com/Shenggan/BCCD_Dataset

The dataset contains 364 images with three object classes. Original annotations are provided in Pascal VOC XML format and were converted into YOLO format.

Dataset split:

| Split | Images | Percentage |
|---|---:|---:|
| Training | 254 | 69.8% |
| Validation | 73 | 20.1% |
| Testing | 37 | 10.2% |
| Total | 364 | 100% |

The dataset was split using random seed 42. The test set was kept separate from training and validation.

*Note: Add the exact number of annotated objects per split and per class after checking your converted labels.*

## 3. Data Preparation

The original Pascal VOC XML annotations were converted into YOLO bounding-box format.

Each annotation contains:

- Class ID
- Normalized bounding-box center coordinates
- Normalized bounding-box width and height

Class mapping:

| Class | ID |
|---|---:|
| RBC | 0 |
| WBC | 1 |
| Platelets | 2 |

The dataset was divided into training, validation, and testing subsets. The same split was used for both training experiments.

## 4. Model and Training

**Model:** YOLO11n  
**Initial weights:** yolo11n.pt (pretrained)  
**Framework:** Ultralytics  
**Hardware:** NVIDIA Tesla T4 GPU

Baseline configuration:

| Parameter | Value |
|---|---:|
| Image size | 640 × 640 |
| Epochs | 50 |
| Batch size | 8 |
| Initial learning rate | 0.01 |
| Random seed | 42 |
| Early stopping patience | 15 |

Baseline training time: approximately 4.95 minutes.

The best checkpoint was selected based on validation performance.

## 5. Controlled Experiment

A controlled experiment was conducted to investigate the effect of image resolution.

The only changed parameter was image size:

- Baseline: 640 × 640
- Experiment: 512 × 512

All other training parameters, dataset splits, model architecture, and pretrained weights were kept unchanged.

### Results

| Metric | Baseline (640) | Experiment (512) |
|---|---:|---:|
| Precision | 0.8666 | 0.8782 |
| Recall | 0.8746 | 0.8544 |
| mAP@0.50 | 0.9303 | 0.9229 |
| mAP@0.50:0.95 | 0.6409 | 0.6305 |
| Training time | 4.95 min | 3.53 min |

Reducing the image size decreased training time by approximately 28.7%. Precision increased slightly, while recall and both mAP metrics decreased.

The baseline model was selected as the final model because it achieved better overall detection performance.

## 6. Final Test Evaluation

The selected baseline checkpoint was evaluated on the held-out test dataset.

### Test results

| Metric | Result |
|---|---:|
| Precision | 0.8415 |
| Recall | 0.9092 |
| mAP@0.50 | 0.9334 |
| mAP@0.50:0.95 | 0.6499 |

Evaluation settings:

- Image size: 640
- Confidence threshold: 0.001
- IoU threshold: 0.7

Prediction visualization was generated separately using a confidence threshold of 0.25.

## 7. Failure Analysis

Three main limitations were identified from the reviewed test predictions.

**1. Overlapping RBCs**

The model had difficulties distinguishing individual RBCs when multiple cells overlapped or were positioned close together. This was particularly noticeable because RBCs were more numerous than the other classes.

**2. Platelet detection ambiguity**

In two reviewed images, platelets appeared inside or very close to RBCs. Their small size and visual overlap made them harder to distinguish and classify.

**3. Object separation in crowded regions**

The model generally produced well-positioned bounding boxes, but separating individual RBC instances in dense regions remained challenging.

Potential improvements include increasing the diversity of training examples containing overlapping cells, improving annotation quality, and experimenting with higher image resolution.

## 8. Limitations and Future Improvements

Possible directions for further work include:

- Increasing the number of training examples.
- Improving annotations for overlapping objects.
- Investigating class-specific detection performance.
- Experimenting with higher image resolutions.
- Evaluating alternative YOLO architectures.

## 9. Reproducibility

The project notebook contains the complete workflow:

1. Dataset acquisition.
2. Annotation conversion.
3. Dataset splitting.
4. Baseline model training.
5. Controlled experiment.
6. Validation comparison.
7. Final test evaluation.
8. Prediction visualization.

To reproduce the project, install the dependencies listed in `requirements.txt`, run the notebook in Google Colab, and follow the dataset preparation instructions.

## 10. Conclusion

The project successfully fine-tuned a pretrained YOLO11n model for blood-cell object detection.

The final model achieved 0.9334 mAP@0.50 and 0.9092 recall on the held-out test set. The controlled experiment demonstrated the trade-off between image resolution, training efficiency, and detection performance.

The results show that transfer learning with YOLO11n can provide strong detection performance on the BCCD dataset, although overlapping cells and small platelets remain challenging cases.
