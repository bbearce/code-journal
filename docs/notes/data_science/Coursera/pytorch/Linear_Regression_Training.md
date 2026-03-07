# Linear_Regression_Training


![Images/linear_regression_training_1](Images/linear_regression_training_1.png)

In this video, we will go over the process of learning parameters for linear regression. This is called training. In this module, we will review what is a dataset, the noise assumption, provide an overview of training. We use a dataset of examples. In this case, we have n points with x and y values. We will use these examples to learn the linear relationship, or line, between x and y. When x is only one dimension, linear regression is sometimes referred to as simple linear regression. Imagine this set with many thousands of ordered pairs. Corresponding x-y coordinates are marked with the same subscript, linking them together. Subscripts range from n equals 1 to n with n defining the set size. Commonly, you will encounter datasets organized as tensors.

![Images/linear_regression_training_2](Images/linear_regression_training_2.png)

Example of simple linear regression datasets include predicting housing sizes, giving the size of the house. The variable y is the house price, and x is the size. Predicting stock prices using interest rates. In this case, y is the stock price, and x is the interest rate. Fuel economy of cars give horsepower. Y is the fuel economy, and x is the horsepower.


![Images/linear_regression_training_3](Images/linear_regression_training_3.png)


Even if the linear assumption is correct, there is always some error. We take this into account by assuming a small random value is added to the point on the line. This is called noise. For linear regression, the particular type of noise is Gaussian. 

![Images/linear_regression_training_4](Images/linear_regression_training_4.png)


The figure on the left shows the distribution of the noise. The horizontal axis shows the value added, and the vertical axis illustrates the probability that the value will be added. 

![Images/linear_regression_training_5](Images/linear_regression_training_5.png)

Usually, a small positive value is added

![Images/linear_regression_training_6](Images/linear_regression_training_6.png)

...or a small negative value. Sometimes large values are added

![Images/linear_regression_training_7](Images/linear_regression_training_7.png)

...but for the most part, the values added are near zero.

![Images/linear_regression_training_8](Images/linear_regression_training_8.png)

The more significant the standard deviation, or the more dispersed the distribution is, the more the samples deviate from the line.

![Images/linear_regression_training_9](Images/linear_regression_training_9.png)

In linear regression, we plot the points on the Cartesian plane. We would like to come up with a linear function for x that can best represent the points. In this example, the line does not do a good job. This line does a slightly better job at fitting the points. Finally, this line does the best at fitting all the points. A more systematic way of finding the best line can be determined by minimizing a function.

![Images/linear_regression_training_10](Images/linear_regression_training_10.png)

The following is the Average Loss, Mean Squared Error, or COST function. It is a function of the slope and bias. As we plug in different slope and biases, we get different values. It turns out the line with the best fit has the smallest value for this function. Now let's see how to minimize this cost. 

