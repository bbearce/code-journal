# Introduction

> Source: [fast.ai](https://course.fast.ai/)

Fastai is based on pyTorch and if Keras simplifies Tensorflow, Fastai simplifies pyTorch. 

## Getting Started

### What is a GPU?
GPUs (Graphics Processing Units) are specialized computer hardware originally created to render images at high frame rates (most commonly images in video games). Since graphics texturing and shading require more matrix and vector operations executed in parallel than a CPU (Central Processing Unit) can reasonably handle, GPUs were made to perform these calculations more efficiently.

### Why a GPU?
It so happens that Deep Learning also requires super fast matrix computations. So researchers put two and two together and started training models in GPU’s and the rest is history.

Deep Learning really only cares about the number of Floating Point Operations (FLOPs) per second. GPUs are highly optimized for that.

Try running this inside a Jupyter Notebook:

Cell [1]:
```python
import torch
t_cpu = torch.rand(500,500,500)
%timeit t_cpu @ t_cpu

1 loop, best of 3: 1.78 s per loop
```

Cell [2]:
```python
t_gpu = torch.rand(500,500,500).cuda()
%timeit t_gpu @ t_gpu

1000 loops, best of 3: 15.4 ms per loop
```

If you would like to train anything meaningful in deep learning, a GPU is what you need - specifically an NVIDIA GPU.

Why NVIDIA?
We recommend you to use an NVIDIA GPU since they are currently the best out there for a few reasons:

Currently the fastest

Native Pytorch support for CUDA

Highly optimized for deep learning with cuDNN



