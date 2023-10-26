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
$Dis(x_1, x_2) = \sqrt{(34-30)^2}$
> n here is attributes and the 1 and 2 are neighbors 1 and 2.

2 independent variables (age and income):  
Customer1 = {age: 34, income: 190}  
Customer2 = {age: 30, income: 200}  
$Dis(x_1, x_2) = \sqrt{(34-30)^2+(190-200)^2}$


### So, how do we choose the right K? 
* A low value of K causes a highly complex model and causes overfitting.
* A high value is too general.
* We're not sure yet... loop over k and test accuracy

## Classification Accuracy
Compare predictions $y$ and actual $\hat{y}$ of test data set.

### Jaccard index
$J(y, \hat{y}) = \frac{|y\cap\hat{y}|}{|y\cup\hat{y}|} = \frac{|y\cap\hat{y}|}{|y|+|\hat{y}|-|y\cap\hat{y}|}$

### F1-score
* Precision = TP / (TP + FP)
* Recall =  TP / (TP + FN)
* F1-score = 2x(prc x rec)/(prc + rec)

### Log Loss
Probability of label versus label itself. Only for binary output. This is related to logistic regression.

$LogLoss = \frac{1}{n}\sum_{i=1}^{i}(y\log(\hat{y}) +(1-y)*\log(1-\hat{y}))$

Looks like infinity towards 0 for x and 0 for correct prediction with 100% confidence.
```bash
(y)
00|.
  | .
  | .
  | .
  |  .
  |   .
  |      .
  |            .
 0|                         .
 ------------------------------(x)
```


# K-NN Code Example
[ML0101EN-Clas-K-Nearest-neighbors-CustCat.ipynb](JupyterNotebooks/ML0101EN-Clas-K-Nearest-neighbors-CustCat.ipynb)

```bash
# Setup Environment
cd ~/Desktop; mkdir temp; cd temp; pyenv activate venv3.10.4;
# cd ~/Desktop; rm -r temp; # To remove
```
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from sklearn import preprocessing, metrics
from sklearn.model_selection import train_test_split
from sklearn.neighbors import KNeighborsClassifier

df = pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-ML0101EN-SkillsNetwork/labs/Module%203/data/teleCust1000t.csv')
df.head()
# classes in dataset (customer category)
key = {'1':'Basic Service','2':'E Service','3':'Plus Service','4':'Total Service'}
df['custcat'].value_counts()
# normalize the data
X = df[['region', 'tenure','age', 'marital', 'address', 'income', 'ed', 'employ','retire', 'gender', 'reside']] .values  #.astype(float)
y = df['custcat'].values; y[0:5]
X = preprocessing.StandardScaler().fit(X).transform(X.astype(float)); X[0:5]
# train/test/split
X_train, X_test, y_train, y_test = train_test_split( X, y, test_size=0.2, random_state=4)
print ('Train set:', X_train.shape,  y_train.shape)
print ('Test set:', X_test.shape,  y_test.shape)
# k-nearest neighbor
k = 4
# train model and predict  
neigh = KNeighborsClassifier(n_neighbors = k).fit(X_train,y_train); neigh
# predicting
yhat = neigh.predict(X_test); yhat[0:5]
print("Train set Accuracy: ", metrics.accuracy_score(y_train, neigh.predict(X_train)))
print("Test set Accuracy: ", metrics.accuracy_score(y_test, yhat))
# practice
k=6 # do over with this k
neigh = KNeighborsClassifier(n_neighbors = k).fit(X_train,y_train); neigh
yhat = neigh.predict(X_test); yhat[0:5]
print("Train set Accuracy: ", metrics.accuracy_score(y_train, neigh.predict(X_train)))
print("Test set Accuracy: ", metrics.accuracy_score(y_test, yhat))
```
> metrics.accuracy_score is jaccard_score (jaccard index)
```python
# try even more Ks
Ks = 10
mean_acc = np.zeros((Ks-1))
std_acc = np.zeros((Ks-1))

for n in range(1,Ks):
    #Train Model and Predict  
    neigh = KNeighborsClassifier(n_neighbors = n).fit(X_train,y_train)
    yhat=neigh.predict(X_test)
    mean_acc[n-1] = metrics.accuracy_score(y_test, yhat) # jaccards
    std_acc[n-1]=np.std(yhat==y_test)/np.sqrt(yhat.shape[0]) # standard error
    # https://en.wikipedia.org/wiki/Standard_error

mean_acc
std_acc
# plot
plt.plot(range(1,Ks),mean_acc,'g')
plt.fill_between(range(1,Ks),mean_acc - 1 * std_acc,mean_acc + 1 * std_acc, alpha=0.10)
plt.fill_between(range(1,Ks),mean_acc - 3 * std_acc,mean_acc + 3 * std_acc, alpha=0.10,color="green")
plt.legend(('Accuracy ', '+/- 1xstd','+/- 3xstd'))
plt.ylabel('Accuracy ')
plt.xlabel('Number of Neighbors (K)')
plt.tight_layout()
plt.show()
# best accuracy
print( "The best accuracy was with", mean_acc.max(), "with k=", mean_acc.argmax()+1) 
```

