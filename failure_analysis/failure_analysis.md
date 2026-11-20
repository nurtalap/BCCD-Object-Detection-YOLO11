# Failure Analysis

Three main limitations were observed during the qualitative review of test predictions.

### 1. Overlapping RBCs

The model sometimes had difficulty distinguishing individual RBCs when multiple cells overlapped or were positioned close together. This was particularly noticeable because RBCs were more numerous than the other classes.

### 2. Platelet Detection Ambiguity

In two reviewed images, platelets appeared inside or very close to RBCs. Their small size and visual overlap made them harder to distinguish and classify.

### 3. Object Separation in Crowded Regions

The model generally produced well-positioned bounding boxes, but separating individual RBC instances in dense regions remained challenging.

### Potential Improvements

- Increase the diversity of training examples containing overlapping cells.
- Improve annotation quality for crowded regions.
- Experiment with higher image resolution.
- Investigate class-specific detection performance.

These observations are based on qualitative inspection of five test prediction examples.
