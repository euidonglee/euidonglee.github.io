---
title: "[Optical Recognition of Handwritten Digits] 1. Feature Engineering"
date: 2020-06-25 22:20:32 -0400
categories: Data_Science
---
## Observing the features
The features are integer values from 0 to 16, for each pixel $$p_ij$$.

## Dataset
[Optical Recognition of Handwritten Digits Data Set](https://archive.ics.uci.edu/ml/datasets/Optical+Recognition+of+Handwritten+Digits)

The famous MNIST dataset is compressed from 28 * 28 pixels to 8x8 pixels, adding some optical distortion to the image. The digits are written by a total of 43 writers.

![Snipshot of our dataset](/assets/images/optical_recognition_0.png)