# Simple Dataset

```python
import torch
from torch.utils.data import Dataset
from torchvision import transforms
```

## Datasets
```python
class toy_set(Dataset):
    def __init__(self, length=100, transform=None):
        self.x = 2 * torch.ones(length, 2)
        self.y = torch.ones(length, 1)
        
        self.len = length
        self.transform = transform
        
    def __getitem__(self, index):
        sample = self.x[index], self.y[index]
        if self.transform:
            sample = self.transform(sample)
        return sample
        
    def __len__(self):
        return self.len
```

```python
dataset = toy_set()
len(dataset) # 100
dataset[0] # (tensor([2., 2.]), tensor([1.]))

for i in range(3):
    x,y=dataset[i]
    print(i,'x:',x,'y:',y)

# 0 x: tensor([2., 2.]) y: tensor([1.])
# 1 x: tensor([2., 2.]) y: tensor([1.])
# 2 x: tensor([2., 2.]) y: tensor([1.])
```

## Transforms
Create cllable classes that transform datasets.
```python
class add_mult(object):
    
    def __init__(self, addx=1, muly=1):
        self.addx = addx
        self.muly = muly
    
    def __call__(self, sample):
        x = sample[0]
        y = sample[1]
        x = x + self.addx
        y = y * self.muly
        sample = x, y
        return sample
```

```python
# First lets apply to a dataset with None for transform
dataset = toy_set()
dataset[0] # dataset[0]
a_m=add_mult()
x_,y_ = a_m(dataset[0])

# Second lets set datasets transform object
dataset_ = toy_set(length=100, transform=a_m)
dataset_[0] # (tensor([3., 3.]), tensor([1.]))
```

## Transforms Compose
In this case, we create the class “mult” that will multiply all the elements of a tensor by the value mul. 

```python
class mult(object):
    
    def __init__(self, mul=100):
        self.mul = mul
    
    def __call__(self, sample):
        x = sample[0]
        y = sample[1]
        x = x * self.mul
        y = y * self.mul
        sample = x, y
        return sample
```


We create a transforms Compose object. In the constructor, we place a list. The first element of the list is the constructor for the first transform; the second element of the list is the constructor for the second transform. We can apply the transform on the data directly. The function takes the input elements of the dataset, applies the first transform, applies the second transform, and returns the output as a tuple containing the tensors. We can apply the compose object directly in the dataset constructor Each time we retrieve a sample, the original tensor is passed to the compose object, the first transform is applied, then the second transform is applied. 
```python
# Compose(transforms: List[Callable])
data_transform = transforms.Compose([add_mult(), mult()])
dataset_ = toy_set(length=100, transform=data_transform)
dataset_[0] # (tensor([300., 300.]), tensor([100.]))
```