# Artificial_Neural_Networks

## Gradient Descent
### Cost Function
We fit a line to data. If we nail the weight $w$ then we have 0 loss. There is much loss if $w$ is not ideal and the loss increases exponentially the further we get from the origin with the loss function we defined. We need derivatives or "gradients" so we know if we change $w$ that the loss will be lower. We do this iteravely adjusting learning rate $\eta$. 
![Images/artificial_neural_networks/cost_func_gradient_descent_1.png](Images/artificial_neural_networks/cost_func_gradient_descent_1.png)
![Images/artificial_neural_networks/cost_func_gradient_descent_2.png](Images/artificial_neural_networks/cost_func_gradient_descent_2.png)
![Images/artificial_neural_networks/cost_func_gradient_descent_3.png](Images/artificial_neural_networks/cost_func_gradient_descent_3.png)
![Images/artificial_neural_networks/cost_func_gradient_descent_4.png](Images/artificial_neural_networks/cost_func_gradient_descent_4.png)
![Images/artificial_neural_networks/cost_func_gradient_descent_5.png](Images/artificial_neural_networks/cost_func_gradient_descent_5.png)
![Images/artificial_neural_networks/cost_func_gradient_descent_6.png](Images/artificial_neural_networks/cost_func_gradient_descent_6.png)
![Images/artificial_neural_networks/cost_func_gradient_descent_7.png](Images/artificial_neural_networks/cost_func_gradient_descent_7.png)

## Backpropogation
How do we train and optimize weights and biases. Training is done in a supervised setting with a ground truth.

1. Calculate error between GT and estimated output. Denote this $E$.
2. Propogate this error back into the network and update each weight and bias as per the following equation:

    $w_i \rightarrow w_i - \eta * \frac{\delta E}{\delta w_i}$

    $b_i \rightarrow b_i - \eta * \frac{\delta E}{\delta b_i}$


3. Backpropogation

    For a two neuron network we can calculate $E$ for an observation. For multiple observations the equation is as follows:

    $E = \frac{1}{2m} \sum_{i=1}^{m} (T_i - a_{2,i})^2$

    ![Images/artificial_neural_networks/cost_func_gradient_descent_8.png](Images/artificial_neural_networks/cost_func_gradient_descent_8.png)

    > This is a total aggregate error.

4. Chain Rule
    
    Notice how $E$ is not directly a function of $w_2$. Through a **proof** we can show that updating weights and biases is as follows:

    ![Images/artificial_neural_networks/updating_w2_1.png](Images/artificial_neural_networks/updating_w2_1.png)
    ![Images/artificial_neural_networks/updating_w2_2.png](Images/artificial_neural_networks/updating_w2_2.png)

    ![Images/artificial_neural_networks/updating_b2.png](Images/artificial_neural_networks/updating_b2.png)
    
    ![Images/artificial_neural_networks/updating_w1_1.png](Images/artificial_neural_networks/updating_w1_1.png)
    ![Images/artificial_neural_networks/updating_w1_2.png](Images/artificial_neural_networks/updating_w1_2.png)
    
    ![Images/artificial_neural_networks/updating_b1.png](Images/artificial_neural_networks/updating_b1.png)

## Vanishing Gradient

## Activation Functions