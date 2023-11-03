# Logistic Regression

## Intro to Logistic Regression
### Overview
Logistic regression is a statistical and machine learning technique for classifying records of a datastr based on the values of the input fields.

 Let's say we have a telecommunication dataset that we'd like to analyze in order to understand which customers might leave us next month. This is historical customer data where each row represents one customer. Imagine that you're an analyst at this company and you have to find out who is leaving and why? You'll use the dataset to build a model based on historical records and use it to predict the future churn within the customer group.

In logistic regression, we use one or more independent variables such as tenure, age, and income to predict an outcome, such as churn, which we call the dependent variable representing whether or not customers will stop using the service.  

Logistic regression is analogous to linear regression but tries to predict a categorical or discrete target field instead of a numeric one. In linear regression, we might try to predict a continuous value of variables such as the price of a house, blood pressure of a patient, or fuel consumption of a car. But in logistic regression, we predict a variable which is binary such as yes/no, true/false, successful or not successful, pregnant/not pregnant, and so on, all of which can be coded as zero or one.

In logistic regression independent variables should be continuous. If categorical, they should be dummy or indicator coded. This means we have to transform them to some continuous value.

Please note that logistic regression can be used for both binary classification and multi-class classification.

## Applications
* To predict the probability of a person having a heart attack within a specified time period
* Predict the chance of mortality in an injured patient
* Predict whether a patient has a given disease such as diabetes
* Predict the likelihood of a customer purchasing a product or halting a subscription as we've done in our churn example
* Probability of failure of a given process, system or product
* Predict the likelihood of a homeowner defaulting on a mortgage

>  Notice that in all these examples not only do we predict the class of each case, we also measure the probability of a case belonging to a specific class.

* one
    - inner 1
    - inner 2
* two


The question is, when should we use logistic regression?  

1. If your data is binary:  
    - 0/1, YES/NO, True/False  
2. If you need probabalistic results:  
3. When you need a linear decision boundary:  
    - The decision boundary of a logistic regression is a line or a plane or a hyper plane  
    - A classifier will classify all the points on one side of the decision boundary as belonging to one class and all those on the other side as belonging to the other class.  
    - For example, if we have just two features and are not applying any polynomial processing we can obtain an inequality like $\theta_0 + \theta_1x_1 + \theta_2x2 > 0$, which is a half-plane easily plottable.  
4. You need to understand the impact of a feature.
   - You can select the best features based on the statistical significance of the logistic regression model coefficients or parameters.
   - That is, after finding the optimum parameters, a feature X with the weight $\theta_1$ close to 0 has a smaller effect on the prediction than features with large absolute values of $\theta_1$.

![logistic_regression_1.jpg](Images/logistic_regression_1.jpg)

* X is our dataset in the space of real numbers of m by n:  $X\epsilon \mathbb{R}^{m \times n}$  
* Y is the class that we want to predict: $y\epsilon\{0,1\}$, which can be either 0 or 1  
* Ideally, a logistic regression model, so-called Y hat, can predict that the class of the customer is one, given its features X: $\hat{y} = P(y=1|x)$  
 It can also be shown quite easily that the probability of a customer being in class zero can be calculated as one minus the probability that the class of the customer is one: $P(y=0|x)= 1 - P(y=1|x)$

## Logistic Regression vs Linear Regression

* We will learn the difference between linear regression and logistic regression.

* We go over linear regression and see why it cannot be used properly for some binary classification problems.

* We also look at the sigmoid function, which is the main part 
of logistic regression. 

Recall we want to predict the class of each customer $\hat{y} = P(y=1|x)$ and also the probability of each sample belonging to a class.

> y is the label's vector, also called **actual values**, that we would like to predict, and $\hat{y}$ is the vector of the predicted values by our model.

### Let's Try Linear Regression
Let's use age to predict chrun (categorical value). We need to plot churn versus age which will have two horizontal areas of points for churn=0 and churn=1.

![logistic_regression_2](Images/logistic_regression_2.jpg)

* Prediction can be represented as $\hat{y}=\theta_0 + \theta_1x_1$
* Where $\theta^T=[\theta_0,\theta_1,...,\theta_n]$
* Formally the line is $\theta^TX=\theta_0+\theta_1x_1+\theta_2x_2+...+\theta_nx_n$
* X (feature set) can be represented as: $X = \left[ \begin{array}{c} 1 \\ x_1 \\ x_2 \\ \end{array} \right]$

Seen here we can estimate the regression line and predict churn with $\theta^TX$. Let's try with 13 for $x_1$.

![logistic_regression_3.jpg](Images/logistic_regression_3.jpg)

Then set a threshold of 0.5 for determining 0 or 1. Finally $\hat{y}$ can be represented as a piecewise function to force it to be categorical. In this example $P_1=[13]$ is Class 0. There is one problem here. What is the probability that this customer belongs to class zero?

As you can see, it's not the best model to solve this problem. Also, there are some other issues which verify that linear regression is not the proper method for classification problems. So, as mentioned, if we use the regression line to calculate the class of a point, it always returns a number such as three or negative two, and so on. Then, we should use a threshold, for example, 0.5, to assign that point to either class of zero or one. This threshold works as a step function that outputs zero or one regardless of how big or small, positive or negative the input is. So, using the threshold, we can find the class of a record. Notice that in the step function, no matter how big the value is, as long as it's greater than 0.5, it simply equals one and vice versa. Regardless of how small the value y is, the output would be zero if it is less than 0.5. In other words, there is no difference between a customer who has a value of one or 1,000. The outcome would be one. Instead of having this step function, wouldn't it be nice if we had a smoother line, one that would project these values between zero and one? 

Let's use the **Sigmoid Function**.
![logistic_regression_4.jpg](Images/logistic_regression_4.jpg)

It gives a non-step function approach that gives us the probabilty that a point belongs to a certain class instead of the value of y directly.

Instead of calculating the value of Theta transpose x directly, it returns the probability that a Theta transpose x is very big or very small. It always returns a value between 0 and 1, depending on how large the Theta transpose x actually is. 

Now, our model is $\hat{y}=\sigma(\theta^TX)$, which represents the probability that the output is 1 given x.

#### Sigmoid Function
$\sigma(\theta^TX)=\frac{1}{1+e^{-\theta^TX}}$

When $\theta^T$ goes up $\sigma(\theta^TX)$ tends towards 1 and when small it tends to 0. This is the probability not the class itself.

##### What is the output of our model?  
* $P(Y=1|X)$ - probability of Y=1 given X  
* $P(Y=0|X) = 1-P(Y=1|X)$ - probability of Y=0 given X

Ex:
* $P(Churn=1|income,age)$ = 0.8
* $P(Churn=0|income,age)$ = 1- 0.8 = 0.2

Now we can train our model to set its parameter values in such a way that our model is a good estimate of the probablity of Y given X:  $\sigma(\theta^TX)$ -> $P(y=1|x)$.

* $\sigma$ is model.
* $P(y=1|x)$ and $P(y=0|x)$ are actual probabilities. 

##### How to train this model?
$\sigma(\theta^TX)$ -> $P(y=1|x)$

1. Initialize $\theta$ with random values: $\theta$ = [-1, 2]
2. Calculate model output $\hat{y} = \sigma(\theta^TX)$ for a sample customer.  
   $\hat{y} = \sigma([-1,2]] x [2,5])$ = 0.7 - "2 and 5 are age and income"
3. Compare the output of $\hat{y}$ with the actual output of custom, y, and record it as error. Example: $Error = 1 - 0.7 = 0.3$. 1 Here was Churn and 0.7 was the probability of Chrun from model
4. Calculate the error for all customers. Add up these errors. Total error is the cost of your model and is calculated by your models cost function. The cost function, by the way, basically represents how to calculate the error of the model which is the difference between the actual and the models predicted values. You want low cost (preferrably 0).
5. Cost is high right now. We need to change to get lower cost. We change $\theta$ in such a way to hopefully reduce the total cost. 
6. Go back to step 2. and iterate to get a new cost. We do this until the cost is low enough.

One of the most popular ways is gradient descent. Also, there are various ways to stop iterations, but essentially you stop training by calculating the accuracy of your model and stop it when it's satisfactory. 

### Logistic Regression Training
Objective - $\sigma(\theta^TX)$ -> $P(y=1|x)$

Let's look at the cost function.

Cost($\hat{y}, y$) = $\sigma(\theta^TX) - y$

Usually the square of this equation is used because of the possibility of the negative result and for the sake of simplicity, half of this value is considered as the cost function through the derivative process.

Cost($\hat{y}, y$) = $\frac{(\sigma(\theta^TX) - y)^2}{2}$

> Note this is for a particular sample.

If we want for all samples we need a new equation:  
$J(\theta) = \frac{1}{m} \sum_{i-1}^mCost(\hat{y},y)$

> This confused me for a second as $\theta$ refers to the models parameters. It's the cost across all samples divided by the number of samples. This is also called **mean squared error**. The reason it's a function of $\theta$ is because all this "cost" we are measuring is for a specific set of $\theta$ parameters.

Now, how do we find or set the best weights or parameters that minimize this cost function? The answer is, we should calculate the minimum point of this cost function and it will show us the best parameters for our model.

Although we can find the minimum point of a function using the derivative of a function, there's not an easy way to find the global minimum point for such an equation. Given this complexity, describing how to reach the global minimum for this equation is outside the scope of this video.

So, what is the solution? Well we should find another cost function instead, one which has the same behavior but is easier to find its minimum point. Let's plot the desirable cost function for our model. 

Let's plot the desirable cost function for our model. Recall that our model is y hat. Our actual value is y which equals zero or one, and our model tries to estimate it as we want to find a simple cost function for our model.

This means our model is best if it estimates y equals one. In this case, we need a cost function that returns zero if the outcome of our model is one, which is the same as the actual label. And the cost should keep increasing as the outcome of our model gets farther from one. And cost should be very large if the outcome of our model is close to zero. 

We can see that a function $-log(\hat{y})$ looks similar to how we want to model this:

![logistic_regression_5.jpg](Images/logistic_regression_5.jpg)

Recall the derivative of the cost function was hard to calculate. So instead we define as:

\[
Cost(\hat{y},y) = 
\begin{cases}
  -log(\hat{y}) & \text{if } y = 1 \\
  -log(1-\hat{y}) & \text{if } y = 0
\end{cases}
\]

and redefine $J(\theta)$ as:  
$J(\theta) = \frac{1}{m} \sum_{i-1}^m y^i log(\hat{y}^i) + (1-y^i) log(1-\hat{y}^i)$

### Gradient Descent To Minimize the Cost
Let's assume $\hat{y} = \sigma(\theta_1x_i + \theta_2x_2)$  

and

$J(\theta) = \frac{1}{m} \sum_{i-1}^m y^i log(\hat{y}^i) + (1-y^i) log(1-\hat{y}^i)$
