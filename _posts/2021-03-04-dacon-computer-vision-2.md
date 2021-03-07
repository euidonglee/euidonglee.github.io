---
title: "[DACON 2nd Computer Vision Competition] 2. Modeling & Results"
date: 2021-03-04 22:00:02 -0400
categories: Data_Science
---
## Creating the Resnet50 image classifier
From torchvision we can get the pretrained Resnet50 model:

~~~python
from torchvision.models import resnet50
model = Resnet50Model()
~~~

We finally create our model by adding a linear classifier on top.

~~~python
class Resnet50Model(nn.Module):
    def __init__(self):
        super().__init__()
        self.resnet = resnet50(pretrained = True)
        # 1000: Resnet50 output size, 26: number of alphabets
        self.classifier = nn.Linear(1000, 26)
        
    def forward(self, x):
        x = self.resnet(x)
        x = self.classifier(x)
        return x
~~~

## Creating Resnet50 inputs
From our custom albumentations pipeline, we add a normalization step:

~~~python
# alubmentations pipeline for training data
transform_train = A.Compose([
    A.HorizontalFlip(p=0.5),
    A.VerticalFlip(p=0.5),
    A.RandomBrightnessContrast(p=1),
    A.Rotate(p=1),
    A.Normalize()  # Finally, add normalization. 
])
~~~

We also change our input image shape into (num_channels, H, W).

~~~python
def __getitem__(self, i):
  img = self.transform(image = self.dataset[i])['image']
  img = np.moveaxis(img, -1, 0)  # This is for Resnet50's input shape..
  return img
~~~

## Training the model
We use Adam optimizer and MultiLabelSoftMarginLoss.

~~~python
optimizer = optim.Adam(model.parameters(), lr=1e-5)
criterion = nn.MultiLabelSoftMarginLoss()
~~~

We start training with learning rate 1e-5:

~~~python
local_loss_sum = 0
epoch_loss = 0
model.train()
for epoch in range(load_epoch + 1, num_epochs):
    for batch_id, (data, label) in enumerate(tqdm(train_loader)):
        optimizer.zero_grad()
        data = data.to(device)
        label = label.to(device)
        
        output = model(data)
        
        loss = criterion(output, label)
        loss.backward()
        local_loss_sum += loss.data
        epoch_loss += loss.data
        
        optimizer.step()
        
        if (batch_id+1)%log_interval == 0:
            print("epoch {} batch id {} loss {} lr {}".format(epoch+1, batch_id+1, local_loss_sum / log_interval, optimizer.param_groups[0]['lr']))
            local_loss_sum = 0
    epoch_losses.append(epoch_loss / len(train_loader))
    epoch_loss = 0

plt.plot(epoch_losses)
~~~

![epoch 0-150](/assets/images/dacon_computer_vision_2_0.png)

We get a local minimum 0.03 at epoch 150.

We tune the learning rate to 1e-6 and continue training:

![epoch 0-210](/assets/images/dacon_computer_vision_2_1.png)

We finally stop at loss 0.002 at epoch 210.

## Results
![results](/assets/images/dacon_computer_vision_2_2.png)

Final accuracy of our model was **0.80295**.



