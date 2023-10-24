# Classification
Source: [Machine_Learning_With_Python_IBM](https://www.coursera.org/learn/machine-learning-with-python)

* Supervised learning
* Unknown items into discrete categories
* Multiple inputs (values) to predict a single field (output column

* Decisions trees
* Naive bayes
* Linear discriminant analysis
* k-Nearsest neighbors
* Logistic regression
* Neural networks
* Support vector machines (SVM)


## KNN (K-Nearest Neighbors)
* independent variables: ($x_n$)
* target field: dependent var (y) - might have n classes

Ex:
$x_n$'s = age and income

* Literally cluster a bit...maybe 5 nearest neighbors. k is 5.
* Classify cases based on their similarity to other cases
* Cases that are near each other are said to be neighbors
* Based on similar cases with same class labels are near each other

1. Pick a value for k.
2. Calc distance to unknown case from allcases
3. Select the k-obserservations ,............
4. Predict the response of the unknown data point using the most poilar responsecale from the K-nearest neighbors

Ex:
Customer1 = {age: 34}
Customer2 = {age: 30}

$Dis(x_1, x_2) = \sqrt{\sum_{i=0}^{n}(x_{1i}-x_{2i})^2}$  
$Dis(x_1, x_2) = \sqrt{\sum_{i=0}^{n}(34-30)^2}$

2 independent variables:
$Dis(x_1, x_2) = \sqrt{\sum_{i=0}^{n}(x_{1i}^`-x_{2i}^`)^2+(x_{1i}^{``}-x_{2i}^{``})^2}$  

### So, how do we choose the right K? 
* A low value of K causes a highly complex model and causes overfitting.
* A high value is too general.
* We're not sure yet... loop over k and test accuracy

## Classification Accuracy (little shaky..review)
Compare predictions $y$ and actual $\hat{y}$ of test data set.

### Jaccard index
$J(y, \hat{y}) = \frac{|y\cap\hat{y}|}{|y\cup\hat{y}|} = \frac{|y\cap\hat{y}|}{|y|+|\hat{y}|-|y\cap\hat{y}|}$

### F1-score
* Precision = TP / (TP + FP)
* Recall =  TP / (TP + FN)
* F1-score = 2x(prc x rec)/(prc + rec)

### Log Loss
Probability of label versus label itself. Only for binary output.

$LogLoss = \frac{1}{n}\sum_{i=1}^{i}(y\log(\hat{y}) +(1-y)*\log(1-\hat{y}))$

