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

### Feature 1. 3 x 3 Average kernel
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
