# Fully Connected Neural Network Architecture

* 1 hidden layer is a deep neural network
* Deeper are more accurate but take longer to train
* ReLU is 0 < 0 and linear above 0
* Dropout helps with overfitting

## Lab: Neural Network Rectified Linear Unit -ReLU- vs Sigmoid-v2
[Neural Network Rectified Linear Unit -ReLU- vs Sigmoid-v2](JupyterNotebooks/Neural Network Rectified Linear Unit -ReLU- vs Sigmoid-v2)

## Reading: Training a Neural Network with Momentum and Data Augmentation

* Momentum - optimizes training process  
* Data Augmentation - expands and diversifies the training data  


### Momentum
In standard gradient descent, parameter updates rely solely on the current gradient, which can make learning slow—especially when the loss surface has sharp curves or narrow valleys. Momentum addresses this by incorporating information from previous gradients, creating a 'velocity' that smooths updates and builds speed in consistent directions.

$v_{t} = \gamma v_{t-1} + \eta \nabla J(\theta)$  
$\theta = \theta - v_{t}$

* $v_t$ represents the accumulated volocity (or smoothed gradient)
* $\gamma$ is the momentum coefficient (usually around 0.9)
* $\eta$ is the learning rate, and
* $\nabla J(\theta)$ is the gradient of the loss functoin converning the parameters.

### Data Augmentation

Data augmentation is a powerful technique for reducing overfitting, a common challenge in neural network training where the model performs well on training data but poorly on unseen data. It works by generating new, varied versions of existing training samples through random transformations such as cropping, resizing, flipping, rotating, or adding noise.

In practice, data augmentation is often applied on the fly during training using tools such as tf.image in TensorFlow or torchvision in PyTorch, ensuring each training batch contains fresh variations for improved generalization.