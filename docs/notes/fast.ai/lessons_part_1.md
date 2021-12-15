# Lessons (Part 1)

> Source Notebook [bbearce_colab](https://colab.research.google.com/drive/1sGHM9GDy1izOjiNC9MSkC9Q7vLo5nn08)

## Lesson 1 - Pets

Keyboard shortcuts:  

* ```Ctrl+m b``` makes a new code block  
* ```Shift+Enter``` to run a cell and proceed to next  
* ```Cntl+Enter``` runs a cell and does not proceed to next  

> Key function to remember: ```untar_data``` is an unzip utility

### Transfer Learning

Take a pre-trained model and then further learn for your specific task. In the course we downloaded "resnet34-333f7ec4.pth", which is a re-trained model and then further train it on cats and dogs. The point of this is that this model was previously trained on "image net" which is a giant repository of general images like planes, houses, animals and other objects so that the model is already pretty good at general classification. We will then use the fact that it already knows about "animals" to further train it to detect species of pet or other more specifc things.

> Key function to remember: ```get_image_files``` will get PosixPath file path-to/image-names.jpg...

> Key function to remember: ```get_image_files``` will get PosixPath file path-to/image-names.jpg...


#### Train it
After they setup the resnet model we atually kick offf the learn step:

```python
learn = cnn_learner(data, models.resnet34, metrics=error_rate)
learn.model # prints the architecture
learn.fit_one_cycle(4)
```

When looking at the cycles 4 is a good start. Cycles and Epochs are the same thing. They are passes throught the data. Each pass the model gets more and more accurate. Here is an example:

| epoch | train_loss | valid_loss | error_rate | time  |
|-------|------------|------------|------------|-------|
| 0     | 1.372660   | 0.296747   | 0.096076   | 01:33 |
| 1     | 0.605791   | 0.287795   | 0.092016   | 01:32 |
| 2     | 0.393741   | 0.237194   | 0.078484   | 01:32 |
| 3     | 0.277990   | 0.229392   | 0.066306   | 01:32 |

#### Save it
Now we want to save the model, and this means save the weights or in a classical sense, the coefficients.

```python
learn.save('stage-1')
```

#### Interpret it

We need to look at the loss functions, which tell you how well did you predict what you tried to. Incorrect predictions have high losses and good predictions have low losses
