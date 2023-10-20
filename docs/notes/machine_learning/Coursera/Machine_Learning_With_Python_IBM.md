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

$\hat{y} = \theta_0 + \theta_1 x_1$

A residual error is the difference between y which is the data itself (x,y) from the csv and y(hat), the regression line evaluated at x (x, $\hat{y}$).

The mean of all residual errors shows how poorly the estimation approximates the best regression. This is called the Mean Squared Error:

$MSE = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y_i})^2$

It can be shown that $\theta_0$ and $\theta_1$ are:

$\theta_1 = \frac{\sum_{i=1}^{S} (x_i - \bar{x}) (y_i - \bar{y})}{\sum_{i=1}^{S} (x_i - \bar{x})^2}$

$\theta_0 = \bar{y} - \theta_1 \bar{x}$

where:

* $\bar{x}$ is the mean of all x values
* $\bar{y}$ is the mean of all y values

Let's use code:
```python
import pandas as pd

engine_size = [2.0,2.4,1.5,3.5,3.5,3.5,3.5,3.7,3.7]
cylinders = [4,4,4,6,6,6,6,6,6]
fuel_consumption_comb = [8.5,9.6,5.9,11.1,10.6,10.0,10.1,11.1,11.6]
co2_emissions = [196,221,136,255,244,230,232,255,267]

engines = pd.DataFrame({'engine_size':engine_size,
                        'cylinders':cylinders,
                        'fuel_consumption_comb':fuel_consumption_comb,
                        'co2_emissions':co2_emissions})

engines

theta_0 = 100
theta_1 = 30
x1 = 4

y = theta_0 + theta_1 * 4
y
```

Pros of regressions:
* Very fast
* No parameter tuning
* East to understand, and highly interpretable

### Model Evaluation Approaches
* Train and Test on the Same Dataset
* Train/Test Split

#### Train and Test on the Same Dataset
Identify a testing subset of all data having observations for a given *dependent* variable. We train on all the data then predict on the testing subset and compare predictions of the *dependent* variable from the test set to the observations of the test set for the same variable.

* Training set is *all* data.
* Testing set is *subset* of training.

Let's evaluate the error ($y_i$ being the actual observation and $\hat{y}_j$ being the prediction):

Error = $\frac{1}{n}\sum_{j=1}^{n}|y_i - \hat{y}_j|$

Above is the *mean absolute error* across all values.

This approach has a high *training accuracy* but a low *out-of-sample* accuracy. High training accuracy isn't necessarily a good thing. It results in overfitting. It's important that our models have a high, out-of-sample accuracy.

> The quiz made a point that I want to record. If a model is overly trained to the dataset, it may capture noise and produce a non-generalized model.


#### Train/Test Split
Let's select a train/test split. 

* Training set is *subset* of data.
* Testing set is also a *subset* of data but with no overlap with the training set.
* Training and Test data are *mutually exclusive*, which is a statistical term describing two or more events that cannot happen simultaneously.

This is more realistic for real world problems.

One problem with this approach is the model is highly dependent on which datasets the data is trained and tested.

#### K-Fold Cross-Validation
Another evaluation model called **K-fold cross-validation** can resolve most of these issues. How do you fix a high variation that results from a dataset dependency? Well you average it.

Example:
Take a dataset and make a train\test split where 75% of the data is training and 25% is test. If we have **K equals four folds**, then we make 4 train/test splits. In the **first** fold for example, we use the **first** 25 percent of the dataset for testing and the rest for training. The model is built using the training set and is evaluated using the test set. Then, in the next round or in the **second** fold, the **second** 25 percent of the dataset is used for testing and the rest for training the model. Again, the accuracy of the model is calculated. We continue for all folds. Finally, the result of all four evaluations are averaged. That is, the accuracy of each fold is then averaged, keeping in mind that each fold is distinct, **where no training data in one fold is used in another**. K-fold cross-validation in its simplest form performs multiple train/test splits, using the same dataset where each split is different. Then, the result is average to produce a more consistent out-of-sample accuracy.

> However, going in depth with K-fold cross-validation model is out of the scope for this course.

### Evaluation Metrics in Regression Models
* **M**ean **A**bsolute **E**rror - Average error.  
$MAE =\frac{1}{n}\sum_{j=1}^{n}|y_j-\hat{y}_j|$
* **M**ean **S**quared **E**rror - Focus is geared towards large errors. It accentuate*s worse errors.  
$MSE = \frac{1}{n}\sum_{j=1}^{n}(y_j-\hat{y}_j)^2$
* **R**oot **M**ean **S**quared **E**rror - Square root of MSE. It is interpretable in the same units as response vector or Y units.  
$RMSE = \sqrt{\frac{1}{n}\sum_{j=1}^{n}(y_j-\hat{y}_j)^2}$
* **R**elative **A**bsolute **E**rror - Also known as residual sum of square, takes the MAE and normalizes it.  
$RAE = \frac{\sum_{j=1}^{n}|y_j-\hat{y}_j|}{\sum_{j=1}^{n}|y_j-\bar{y}_j|}$
* **R**elative **S**quared **E**rror - Similar to RAE, but is widely adopted by the data science community as it is used for calculating *R-squared*. *R-squared* is not an error per say but is a popular metric for the accuracy of your model. It represents how close the data values are to the fitted regression line. The higher the *R-squared*, the better the model fits your data.  
$RSE = \frac{\sum_{j=1}^{n}(y_j-\hat{y}_j)^2}{\sum_{j=1}^{n}(y_j-\bar{y}_j)^2}$

$R^2 = 1 - RSE$

Error is difference betwee data points and the trend generated by the algorithm.


