# Lessons (Part 1)

> Source Notebook [bbearce_colab](https://colab.research.google.com/drive/1sGHM9GDy1izOjiNC9MSkC9Q7vLo5nn08)

## Lesson 1 - Pets

Keyboard shortcuts:  

* ```Ctrl+m b``` makes a new code block  
* ```Shift+Enter``` to run a cell and proceed to next  
* ```Cntl+Enter``` runs a cell and does not proceed to next  

### Transfer Learning

Take a pre-trained model and then further learn for your specific task. In the course we downloaded "resnet34-333f7ec4.pth", which is a re-trained model and then further train it on cats and dogs. The point of this is that this model was previously trained on "image net" which is a giant repository of general images like planes, houses, animals and other objects so that the model is already pretty good at general classification. We will then use the fact that it already knows about "animals" to further train it to detect species of pet or other more specifc things.

 
