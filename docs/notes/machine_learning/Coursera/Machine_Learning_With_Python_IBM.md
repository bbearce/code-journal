# Machine Learning With Python
Source: [Machine_Learning_With_Python_IBM](https://www.coursera.org/learn/machine-learning-with-python)

## Supervised and Unsupervised Learning

### Supervised
Overall this is labeled data. Data from a csv is labeled. However be careful. *Supervised* learning is predicting a column for which you already have data but for a new observation or row that you haven't seen yet.

There are two types of supervised techniques.

#### Classification
Predict discrete class label or category

So with a csv like:

```bash
animal_id|eye_size|num_legs|weight|animal
...
```

We could predict the *animal* col as we have that label.

#### Regression
Predict continuous value. So with a csv like:

```bash
car_id|engine_size|cylinders|fuel_consumption|co2_emissions
...
```

If you have many observations, you could attempt to predict *co2_emissions*  for a new observation

### Unsupervised
Overall this is unlabeled data. This could still be from the same csv of data but you would be predicting a new column you don't know about yet. So with a csv like:

```bash
car_id|engine_size|cylinders|fuel_consumption|co2_emissions
...
```

#### Dimension Reduction
Feature selection and dimension reduction reduces redundant features to make the clasification easier.

#### Density Estimation
Simple concept to explore the data to find structue within it.

#### Market Basket Analysis
If you buy a certain group of items, you are likely to buy another group of items.

#### Clustering
Grouping data points and objects that are somehow similar.

* Discover structure
* Summarization
* Anomaly detection

Summary supervised learning has labeled data and unsupervised does not.

## Regression
Take this dataset:
```bash
car_id|engine_size|cylinders|fuel_consumption|co2_emissions
...
```

Can we predict the co2 emissions of the car given all features but co2?

*Dependent* (y) variables are the goal we study. The the dependent variables have to be continuous.

*Independent* (x) variables are the causes of the dependent variable states. Independent variables can be continuous or categorical.

### Simple Regression
One independent variable is used to predict a dependent variable. 

### Multiple Regression
Multiple independent variables are used to predict a dependent variable. 

### Applications of Regression
* Sales forecasting
* Satisfaction analysis
* Price estimation
* Employment income prediction

### Regression Algorithms
* Ordinal regression
* Poisson regression
* Fast forest quantile regression
* Linear, Polynomial, Lasson, Stepwise, Ridge regression
* Bayesian linear regression
* Neural network regression
* Decision forest regression
* Boosted decision tree regression
* KNN (K-nearest neighbors)

### Linear Regression
Return to this this dataset:
```bash
car_id|engine_size|cylinders|fuel_consumption|co2_emissions
...
```

> Remember dependent variables have to be continuous

$y(hat) = \theta_0 + \theta_1 x_1$


A residual error is the difference between y which is the data itself (x,y) from the csv and y(hat), the regression line evaluated at x (x, y(hat)).

The mean of all residual errors shows how poorly the estimation approximates the best regression. This is called the Mean Squared Error:

$MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y_i})^2$

It can be shown that $\theta_0$ and $\theta_1$ are:

$\theta_1 = \frac{\sum_{i=1}^{S} (x_i - x_m) (y_i - y_m)}{\sum_{i=1}^{S} (x_i - x_m)^2}$

$\theta_0 = y_m - \theta_1 x_m$

where:

* $x_m$ is the mean of all x values
* $y_m$ is the mean of all y values


Let's use code:
```python

```

