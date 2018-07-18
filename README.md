# eval-mpii-pose

Scripts for evaluating results on the MPII human pose dataset.

## Input format

Predictions are expected to have the following format:

* Must be a Matlab (.mat) or HDF5 (.h5) file
  - Must have one field, `preds`, which is the joint predictions tensor
  - Tensor size must be [2 x 16 x n] or [n x 16 x 2]
* Must correspond to one of the following subsets: train, val, test
  - See annot/{train,valid,test}.h5 for which examples are in each of
    these subsets

Predictions produced by the following repositories meet these requirements:

* https://github.com/bearpaw/pytorch-pose
* https://github.com/anewell/pose-hg-demo

## Metrics

### PCKh

The PCKh performance metric is the percentage of joints with predicted locations
that are no further than half of the head segment length from the ground truth.

"PCKh total" excludes the pelvis and thorax joints from the calculation,
presumably because they are very easy to predict given that the approximate
person center is provided.

## Scripts

### `evalMPII.m`

Loads predictions from Matlab or HDF5 files and compares them with ground
truth labels to calculate accuracy metrics (eg PCKh). You will want to edit
`getExpParamsNew.m` to add new sets of predictions, and `evalMPII.m` to specify
which predictions to include and which subset (train/val) to use.

### `prepareTestResults.m`

Loads flat test set predictions and prepares them for submission.

## Reference predictions

The `preds/reference` directory contains multiple validation set prediction
files generated by established pose estimation models.
You can compare against these predictions using `evalMPII.m`.

NOTE: Since the reference predictions are for the validation set, they are
_not_ compatible with the `prepareTestResults.m` script.

## File origins

In order to keep evaluation in line with existing work, a lot of files in this
repository were copied verbatim from other sources.

* http://datasets.d2.mpi-inf.mpg.de/andriluka14cvpr/mpii_human_pose_v1_u12_2.zip
  - `annot/mpii_human_pose_v1_u12_1.mat`
* http://human-pose.mpi-inf.mpg.de/results/mpii_human_pose/evalMPII.zip
  - Contents of the `eval/` folder
* https://github.com/anewell/pose-hg-train/tree/master/data/mpii/annot
  - `annot/train.h5`
  - `annot/valid.h5`
  - `annot/test.h5`
