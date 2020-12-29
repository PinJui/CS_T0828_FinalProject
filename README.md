# CS_T0828_FinalProject
Kaggle-Global Wheat Detection Challenge

## Content

- [Introduction](#introduction)
- [Methodology](#methodology)
- [Results](#results)
- [Reference](#reference)

## Introduction
This project requires to train a object detector to detect all wheat heads in the given image.

Given training data is 3373 training images with size 1024x1024

More information can be found in homepage of kaggle competition.

[Kaggle Global Wheat Competition](https://www.kaggle.com/c/global-wheat-detection/overview)

The goal is to pass the baseline of top 10% score on private leaderboard.

## Methodology

The EfficientDet published in CVPR2020 achieves state-of-the-art accuracy with much fewer parameters than previous object detection and since yolov5 is originally prohibited by kaggle due to license problem, we choose it as our network.

First, we train a EfficientDet-D5 model with data augmentation, including:

* Cutmix
* RandomSizeCrop
* HueSaturationValue
* RandomBrightnessContrast
* ToGray
* HorizontalFlip
* VerticalFlip
* Resize
* Cutout

and we train with AdamW(lr=0.0002) for 40 epochs(took around 5 hours on kaggle kernel)

Then use this model to do pseudo labeling.

When making pseudo labels, weighted box fusion is applied.

Then we train the pre-trained D5 model again with pseudo labeled data using the same training config before.

This time we only train for 10 epochs.

After training, we use test time augmentation, including:
* Rotate90
* HorizontalFlip
* VerticalFlip

and by summing all the result, send all the result to weighted box fusion to fuse the predictions.

Then we get the final predictions.

Detail steps can be found in

[Training](https://github.com/PinJui/CS_T0828_FinalProject/blob/main/Training_efficientdet_d5.ipynb)

[Inference](https://github.com/PinJui/CS_T0828_FinalProject/blob/main/efficientdet-pseudo-labeling-tta-wbf.ipynb)

## Results

We get the score using the method above.

![scoreimg](imglink)

## Reference
