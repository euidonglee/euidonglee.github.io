---
title: "[DACON 2nd Computer Vision Competition] 1. Image augmentation"
date: 2021-02-20 22:00:02 -0400
categories: Data_Science
---
## Why image augmentation?
By using ***image augmentation*** we can:
1. Generate more training data.
2. Make our computer vision model to learn structural features(e.g. edges of the alphabet) 

## Albumentations: fast python library for image augmentation
For image augmentation I used the [albumentations](https://github.com/albumentations-team/albumentations) API.
Image augmentation can be easily done with the torchvision-transform style framework of albumentations.

~~~python
from matplotlib import pyplot as plt
import cv2
import albumentations as A

# This will plot transformed image of 'test_data_root/n.png'
def plot_img(n, transform):
    img = cv2.imread(train_data_root + '/{}.png'.format(n), cv2.IMREAD_COLOR)
    transformed_img = transform(image = img)['image']
    plt.figure(figsize = (10, 20))
    plt.subplot(1, 2, 1)
    plt.title('original img')
    plt.imshow(img)
    plt.subplot(1, 2, 2)
    plt.title('transformed img')
    plt.imshow(transformed_img)

transform = A.HorizontalFlip(p=1)
# Plot 10000.png
plot_img(10000, transform)
~~~

![Albumentations HorizontalFlip](/assets/images/dacon_computer_vision_1_0.png)

# HHHI!!
