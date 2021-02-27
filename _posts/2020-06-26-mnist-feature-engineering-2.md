---
title: "[Optical Recognition of Handwritten Digits] 2. Experiment & Results"
date: 2020-06-26 22:20:32 -0400
categories: Data_Science
---
## Choosing the model
There are several ML models for classification tasks such as **Naive Bayes**, **KNN**, **Random Forest**, and **SVM**. What would be the best model for our data?

A **Random Forest** seems ok, but note that decision trees are based on a greedy algorithm. Random forests would classify the images by looking at only a subset of features; but we don't want that. We want an overall view of all the pixels in the image.

SVM works very well in most of ML problems, expecially on datasets with high dimensional space. One disadvantage of SVM is that the computing time can be very high in large datasets. Our dataset has a high dimentional space, and the size is relatively small (3823 instances in training set). Plus, we can classify all the features at once, by finding a single vector. This can give an overall view of an image. So there's no reason not to use SVM.

Let's use ***soft-margin SVM***, because the pixel values of a certain digits can always change, and the result would be disastrous if the model overfits to the training set.

## Experimental environments
|library|version|
|:---|:---|
|numpy|1.18.1|
|scikit-learn|0.22.1|

## Results
First, we measure F-1 scores on several models, on raw feature matrix `X`:

|Classifier|Naive Bayes|KNN|Random Forest|SVM(RBF Kernel, C=10)|
|:---|:---|:---|:---|:---|
|**F-1 score**|0.808952|0.882951|0.941521|0.967708|

By applying ***soft-margin SVM*** by using **C=1.5**, we get:
|C|1.5|
|:---|:---|
|F-1 score|0.970460|





I applied a simple **3 x 3 average kernel** as follows:

![figure 2](/assets/images/optical_recognition_1_1.png)

First, I compute the average of 3 x 3 pixels which are `p_ij`'s neighbors, so that it can hold information about pixels near `p_ij`. But we don't want to lose information about `p_ij`, so finally we add its value.
Before applying the kernel, a zero padding is added to the image, so that we don't lose information about the outmost pixels. Now we replace our old features to new features `X: {x_0 = f(p_00), x_1 = f(p_01), ... , x_63 = f(p_77)}`.

~~~python
# 1. Apply 3x3 average kernel of stride 1.
# 2. Add result to the img.
def kernel3x3(X):
  data = np.ndarray(X.shape)
  for r in range(X.shape[0]):
    img = X[r, :].reshape(8, 8)
    # add size 1 zero padding
    pad_img = np.zeros((10, 10))
    pad_img[1:9, 1:9] = img
    avg = np.ndarray((8, 8))
    for i in range(8):
      for j in range(8):
        avg[i, j] = np.mean(pad_img[i:i+3, j:j+3])
    img = np.add(img, avg)
    data[r]  img.reshape(1, 64)
    
  return data
~~~

Then our features look something like:

![figure 3](/assets/images/optical_recognition_1_2.png)

Now it looks much better. The values in `x_34` contain some information about their neighbors, and the overall shape of digit '5' is maintained.

### Feature 2. Center column mean & Center row mean
![figure 4](/assets/images/optical_recognition_1_3.png)

If we look at figure 3, we can see that some digits have some thick area near the centermost column. With this idea, I computed a new feature `center_col_mean`, which is the mean of all pixel values in columns 3-4.

~~~python
# Mean value of columns 3-4.
# The 'center column density' of a digit
def center_col_mean(X):
  data = np.ndarray((X.shape[0], 1))
  for r in range(X.shape[0]):
    img = X[r, :].reshape(8, 8)
    data[r] = np.mean(img[:, 3:5])  # Compute mean
  
  return data
~~~

With a similar idea I also computed `center_row_mean`.

~~~python
# Mean value of rows 3-4.
# The 'center row density' of a digit
def center_row_mean(X):
  data = np.ndarray((X.shape[0], 1))
  for r in range(X.shape[0]):
    img = X[r, :].reshape(8, 8)
    data[r] = np.mean(img[3:5])  # Compute mean
  
  return data
~~~
