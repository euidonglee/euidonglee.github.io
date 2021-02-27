---
title: "[Optical Recognition of Handwritten Digits] 1. Feature Engineering"
date: 2020-06-25 22:20:32 -0400
categories: Data_Science
---
## Observing the features
The features are integer values from 0 to 16, for each pixel `p_ij`. That is, there are 64 features in our image `X: {x_0 = p_00, x_1 = p_01, ... , x_63 = p_77}`, and 9 classes in `Y: {0, 1, ... , 9}`.
Note that each feature `x_k` in `X` is simply just a value of one pixel. Does it really contain some meaningful information?

Let's observe the following images, which are all classified as '5':

![figure 1](/assets/images/optical_recognition_1_0.png)

If we look at feature `x_36`, which is the value of pixel (4, 4), we can see that the value at each image greatly differs, but the images all need to be classified as '5'.

## Feature enginnering
One problem is that the features are too local; they don't have any information about the neighbors. I tried some new features to solve this problem.

## Feature 1. 3 x 3 Average Kernel
I applied a simple 3 x 3 average kernel as follows:


## Dataset
[Optical Recognition of Handwritten Digits Data Set](https://archive.ics.uci.edu/ml/datasets/Optical+Recognition+of+Handwritten+Digits)

The famous MNIST dataset is compressed from 28 * 28 pixels to 8x8 pixels, adding some optical distortion to the image. The digits are written by a total of 43 writers.

![Snipshot of our dataset](/assets/images/optical_recognition_0.png)
