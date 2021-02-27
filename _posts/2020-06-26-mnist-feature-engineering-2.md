---
title: "[Optical Recognition of Handwritten Digits] 2. Experiment & Results"
date: 2020-06-26 22:20:32 -0400
categories: Data_Science
---
## Choosing the model
There are several ML models for classification tasks such as **Naive Bayes**, **KNN**, **Random Forest**, and **SVM**. What would be the best model for our data?

A **Random Forest** seems ok, but note that decision trees are based on a *greedy algorithm*. Random forests would classify the images by looking at only a subset of features; but we don't want that. We want an overall view of all the pixels in the image.

**SVM** works very well in most of ML problems, especially on datasets with high dimensional space. One disadvantage of SVM is that the computing time can be very high in large datasets. Our dataset has a high dimentional space, and the size is relatively small (3823 instances in training set). Plus, we can classify all the features at once, by finding a single vector. This can give an overall view of an image. So there's no reason not to use SVM.

Let's use ***soft-margin SVM***, because the pixel values of a certain digit can always change, and the result would be disastrous if the model overfits to the training set.

## Experimental environments

|numpy|1.18.1|
|---|---|
|**scikit-learn**|**0.22.1**|

## Results
First, we measure F-1 scores on several models, on raw feature matrix `X`:

|Classifier|Naive Bayes|KNN|Random Forest|SVM(RBF Kernel, C=10)|
|:---|:---|:---|:---|:---|
|**F-1 score**|0.808952|0.882951|0.941521|0.967708|


By applying ***soft-margin SVM*** by using **C=1.5**, we get:

|C|1.5|
|:---|:---|
|**F-1 score**|0.970460|

We take 0.970460 as the baseline performance.


By adding individual features we get:

|Feature|3x3 kernel|center row mean|center col mean|
|:---|:---|:---|:---|
|**F-1 score**|0.974957|0.971570|0.970467|


By adding all features at once we get the final result:
|Feature|3x3 kernel + center row mean + center col mean|
|:---|:---|
|**F-1 score**|**0.980530**|
