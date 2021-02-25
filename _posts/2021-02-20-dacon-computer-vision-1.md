---
title: "[DACON 2nd Computer Vision Competition] 1. Image augmentation"
date: 2021-02-20 22:00:02 -0400
categories: Data_Science
---
## Why image augmentation?
By using ***image augmentation*** we can:
1. Generate more training data.
2. Make our computer vision model to learn structural features(e.g. edges of the alphabet) 

## Albumentations: Fast python library for image augmentation
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

## Setting custom albumentations pipeline
By stacking multiple transforms with probability p, we can create a random-image generating albumentations pipeline.
I used HorizontalFlip, VerticalFlip, RandomBrightnessContrast, and Rotate.

~~~python
transform = A.Compose([
    A.HorizontalFlip(p=0.5),
    A.VerticalFlip(p=0.5),
    A.RandomBrightnessContrast(p=1),
    A.Rotate(p=1)
])
~~~

![Albumentations pipeline](/assets/images/dacon_computer_vision_1_1.png)

## Implementing the pytorch DataLoader
Now by implementing the pytorch DataLoader with our albumentations pipeline, we can load random new images from our training set.

~~~python
class AugmentedDataset(Dataset):
    def __init__(self, start, end, transform=None):
        self.transform = transform
        self.start = start
        self.end = end
    
    # This will load 'start.png ~ end.png' into our Dataset.
    def read_data(self, filepath):
        self.dataset = []
        for i in tqdm(range(self.start, self.end)):
            img = cv2.imread(filepath + '/{0:05d}.png'.format(i), cv2.IMREAD_COLOR)
            self.dataset.append(img)
    
    # Load our labels
    def read_label(self, filepath):
        self.labels = []
        with open(filepath, 'r') as f:
            reader = csv.reader(f)
            next(reader)
            for row in reader:
                self.labels.append(row[1:])
    
    def __getitem__(self, i):
        img = self.transform(image = self.dataset[i])['image']
        label = np.array(self.labels[i]).astype('float32')
        return img, label
    
    def __len__(self):
        return len(self.dataset)

# alubmentations pipeline for training data
transform_train = A.Compose([
    A.HorizontalFlip(p=0.5),
    A.VerticalFlip(p=0.5),
    A.RandomBrightnessContrast(p=1),
    A.Rotate(p=1)
])

train_dataset = AugmentedDataset(0, 100, transform_train)
train_dataset.read_data(train_data_root)
train_dataset.read_label(data_root + '/dirty_mnist_2nd_answer.csv')

train_loader = DataLoader(train_dataset, batch_size = 32)

for batch_id, (img, label) in enumerate(train_loader):
    # Show 0th image in 0th batch
    plt.imshow(img[0])
    break
~~~

![Albumentations pipeline](/assets/images/dacon_computer_vision_1_2.png)
