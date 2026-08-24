# R-CNN Notes

## Why R-CNN mattered

R-CNN was an important bridge between classical computer vision and deep learning-based object detection.

Before R-CNN, CNNs were mainly used for image classification:

```text
image -> class label
```

Object detection requires solving two problems at once:

```text
What is it?  -> classification
Where is it? -> localization
```

R-CNN addressed this by applying a CNN to candidate image regions instead of to the whole image only.

## Core pipeline

```text
Input image
   |
   v
Selective Search
   |
   v
~2,000 region proposals
   |
   v
Resize / warp each region
   |
   v
CNN feature extraction
   |
   +--> SVM classification
   |
   +--> Bounding-box regression
```

A useful way to remember the design is:

- **Selective Search**: proposes regions that may contain objects.
- **CNN**: acts mainly as a feature extractor.
- **SVM**: performs class prediction.
- **Bounding-box regression**: refines the proposed box coordinates.

This makes R-CNN a hybrid pipeline rather than a fully end-to-end neural detector.

## Transfer learning was a major idea

R-CNN reused a CNN pretrained on ImageNet and then fine-tuned it for object detection.

This showed that features learned for large-scale image classification could transfer effectively to detection tasks with much less labeled data.

Conceptually:

```text
ImageNet pretraining
      |
      v
generic visual features
      |
      v
fine-tuning on detection data
```

This is one of the historically important ideas behind modern transfer learning workflows.

## What the CNN produces

In the original R-CNN pipeline, each region is passed through an AlexNet-style CNN and represented by a high-dimensional feature vector.

The CNN is not the final classifier in the detection pipeline. The extracted feature vector is passed to a separate linear SVM.

```text
region -> CNN -> feature vector -> SVM -> class
```

That separation is unusual compared with modern end-to-end detectors, but it reflects how the original system was assembled.

## Why bounding-box regression is separate

Selective Search only provides approximate candidate boxes. A separate regressor predicts corrections to make each box better match the ground-truth object.

Conceptually, the model learns offsets such as:

```text
dx, dy, dw, dh
```

The broader idea of combining classification with localization regression still appears in many modern detectors.

## Main weakness: repeated CNN computation

The biggest architectural problem is that R-CNN runs the CNN independently for every proposal.

```text
proposal 1 -> CNN
proposal 2 -> CNN
...
proposal 2000 -> CNN
```

Many proposals overlap, so convolution is repeatedly computed over nearly the same pixels.

This makes R-CNN expensive in both training and inference.

## How the design evolved

The limitations of R-CNN naturally led to the next two models.

### Fast R-CNN

Instead of running the CNN once per region, Fast R-CNN computes a feature map once for the full image and reuses it for all regions.

```text
image -> CNN once -> shared feature map -> RoI pooling -> predictions
```

The key improvement is **shared convolutional computation**.

### Faster R-CNN

Fast R-CNN still depends on the external Selective Search algorithm. Faster R-CNN replaces that stage with a learnable Region Proposal Network (RPN).

```text
R-CNN
  -> repeated CNN work
Fast R-CNN
  -> shared feature map
Faster R-CNN
  -> learned region proposals with RPN
```

This progression is easier to understand as a sequence of bottleneck removals than as three unrelated models.

## Key takeaway

> R-CNN turns image classification into object detection by applying CNN features to region proposals.

Its historical importance is not that it is a modern production architecture, but that it demonstrated the power of CNN features and transfer learning for object detection.

The most important question to ask after understanding R-CNN is:

> Why recompute the CNN for thousands of overlapping regions when the image-level feature map could be computed once and shared?

That question leads directly to Fast R-CNN.

## References

- Ross Girshick et al., *Rich Feature Hierarchies for Accurate Object Detection and Semantic Segmentation*, CVPR 2014.
