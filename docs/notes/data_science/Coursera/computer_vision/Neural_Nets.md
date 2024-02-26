# Neural Nets and Deep Learning for Image Classification

## Neural Nets

Imagine this box is a decision function we want to learn for classification.
![Images/Neural_Nets](Images/Neural_Nets/neural_nets1.png)


We can also view the problem as trying to approximate the box function using logistic regression:
![Images/Neural_Nets](Images/Neural_Nets/neural_nets2.png)


If we attempt a linear regression turned into a logistic one, we will still get errors for the higher x values.
![Images/Neural_Nets](Images/Neural_Nets/neural_nets3.png)

Shown here:
> Reminder this logistic regression is an activation function
![Images/Neural_Nets](Images/Neural_Nets/neural_nets4.png)

We could draw another line though:

![Images/Neural_Nets](Images/Neural_Nets/neural_nets5.png)

This too perhaps:

![Images/Neural_Nets](Images/Neural_Nets/neural_nets6.png)

Again though not perfect:

![Images/Neural_Nets](Images/Neural_Nets/neural_nets7.png)

However apparently we can make a band pass filter like shape by subtracting one from another to get our prediction to match the desired square pulse from earlier (kind of):

![Images/Neural_Nets](Images/Neural_Nets/neural_nets8.png)

Pretty square pulse we are aiming for:

![Images/Neural_Nets](Images/Neural_Nets/neural_nets9.png)

As you can see with two different neurons perfomring a logistic regression with a different line, we can effectively have our square pulse.

![Images/Neural_Nets](Images/Neural_Nets/neural_nets10.png)

This first layer of neurons and activation functions are called the hidden layer. Each neuron is an artifician neuron. The output layer has one artificial neuron.

![Images/Neural_Nets](Images/Neural_Nets/neural_nets11.png)

Shown below are the activation outputs. You can see red and blue colors but that is not the output of the activation layers. Imagine the dots are black and the red and blue are just letting us know the ground truth. The final layer that fits a black line (decision boundary) is what the output layer has to figure out.

![Images/Neural_Nets](Images/Neural_Nets/neural_nets12.png)


![Images/Neural_Nets](Images/Neural_Nets/neural_nets13.png)

Review that all these logistic regressions are a series of weights and biases that can be learned. These are free forward neural networks and are also called cully connected networks

![Images/Neural_Nets](Images/Neural_Nets/neural_nets14.png)

With many neurons we can create complex decision boundaries.
![Images/Neural_Nets](Images/Neural_Nets/neural_nets15.png)


![Images/Neural_Nets](Images/Neural_Nets/neural_nets16.png)


![Images/Neural_Nets](Images/Neural_Nets/neural_nets17.png)
