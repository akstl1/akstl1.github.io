---
layout: post
title: Heart Disease Classification
image: "/posts/Park2.jpg"
tags: [Python, Classification]
---

# Insert Intro Here

---
## Discover

### Load Relevant Libraries for the project

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.ensemble import RandomForestClassifier
from sklearn.utils import shuffle
from sklearn.model_selection import train_test_split
from sklearn.metrics import confusion_matrix,plot_confusion_matrix,accuracy_score,precision_score,recall_score,f1_score
from sklearn.preprocessing import OneHotEncoder,MinMaxScaler
from sklearn.inspection import permutation_importance

from sklearn.neighbors import KNeighborsClassifier
from sklearn.feature_selection import RFECV

from sklearn.linear_model import LogisticRegression

```

### Load Data

First, the Parkinson's dataset is loaded. This data was gathered from Kaggle (https://www.kaggle.com/nidaguler/parkinsons-data-set).

```python
data = pd.read_csv('../input/parkinsons-data-set/parkinsons.data')

```

### Prepare Data

The dataset used for this project, provided by Max Little of Oxford, was cleaned/formatted prior to it being posted on Kaggle for public use. Luckily, that means all we have to do is drop the 'name' column since it only contains unique identifiers/keys. This is done in the next section.

### Perform EDA

First, we can drop the 'name' column as mentioned above. 
```python
data.drop('name',axis=1,inplace=True)
```

Next, we can get the head of the data to see the general data formatting and structure 
```python
data.head()
```
From the head call, we see that there are 24 columns and each of them is numeric. Glancing quickly at the columns we can see the columns are not all on the same scale, so some scaling will need to take place before analysis can occur.

We can also look to see if there are any null values in our data.

```python
data.info()

```

The info call shows that each column has 195 non-null values, so there is no need to analyze further for rows with incomplete data.

Before splitting the data for analysis, we want to make sure the data is shuffled. If the data was ordered in a particular way to start, shuffling will help ensure that the train/test data sets are more likely to be representative of the whole data set.
```python
data = shuffle(data,random_state=42)
```

As noted by the original authors, many of the people tested had Parkinson's disease. We want to make sure that our data isn't too imbalanced, or else further data preparation may be needed to account for such imbalance. We do this by determining the percent of recordings that had a positive status for Parkinson's Disease.

```python
data['status'].value_counts(normalize=True) 
```

Running the above code, we find that 75% of rows have a positive status. Though this indicates that there are more positive instances, there should be enough instances of both positive and negative results to continue with the analysis without further preparation.

### Split Data for Analysis

```python
X=data.drop('status',axis=1)
y=data['status']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

### Establish A Baseline

```python
scaler = MinMaxScaler()

X_train_lr = pd.DataFrame(scaler.fit_transform(X_train),columns=X_train.columns) #return as df
X_test_lr = pd.DataFrame(scaler.transform(X_test),columns=X_test.columns) #return as df
```

```python
clf_lr = LogisticRegression(random_state=42,max_iter=1000)

feature_selector = RFECV(clf_lr, cv=5)

fit=feature_selector.fit(X_train,y_train)

optimal_feature_count = feature_selector.n_features_
print(f'the optimal number of features is: {optimal_feature_count}')

X_train_lr = X_train_lr.loc[:,feature_selector.get_support()]
X_test_lr = X_test_lr.loc[:,feature_selector.get_support()]

#understand how much better it is to have 2 vars than any other #

plt.plot(range(1,len(fit.grid_scores_)+1),fit.grid_scores_,marker='o')
#range used so x axis goes from 1 to 4 features rather than starting at 0
#second fit is the scores for each number of vars
plt.ylabel("model score")
plt.xlabel('number of features')
plt.title(f'Feature Selection using RFE \n Optimal number of features is {optimal_feature_count} (at score of {round(max(fit.grid_scores_),3)})')
plt.tight_layout()
plt.show()
```
<img src="/img/parkinsonsLR.png" >

```python
clf=LogisticRegression(random_state=42,max_iter=1000)
clf.fit(X_train_lr,y_train)
```


```python
y_pred_class = clf.predict(X_test_lr)
y_pred_proba = clf.predict_proba(X_test_lr)[:,1]


confusion_matrix(y_test, y_pred_class)

plot_confusion_matrix(clf,X_test_lr,y_test,cmap='coolwarm')
```

<img src="/img/parkinsonsLR_Confusion_matrix.png" >


```python
#Accuracy ((# of correct classifactions out of all attempted classifications))

accuracy_score(y_test,y_pred_class)

# Precision (of all observations that were predicted +, how many were actually +)
precision_score(y_test,y_pred_class)

#Recall (of all + observations, how many did we predict as +)
recall_score(y_test,y_pred_class)

#f1 score (harmonic mean of recall and precision)
f1_score(y_test,y_pred_class)
```

When running the above code, we get an f1 score of 0.9589.....

1. KNN
2. Random Forest

## Model Development

### KNN Model Analysis

```python
data.describe()
```

No categorical vars to encode!

```python
scaler = MinMaxScaler()

X_train_knn = pd.DataFrame(scaler.fit_transform(X_train),columns=X_train.columns) #return as df
X_test_knn = pd.DataFrame(scaler.transform(X_test),columns=X_test.columns) #return as df
```


```python
from sklearn.ensemble import RandomForestClassifier


clf = RandomForestClassifier(random_state=42)

feature_selector = RFECV(clf, cv=5)

fit=feature_selector.fit(X_train_knn,y_train)

optimal_feature_count = feature_selector.n_features_
print(f'the optimal number of features is: {optimal_feature_count}')

X_train_knn = X_train_knn.loc[:,feature_selector.get_support()]
X_test_knn = X_test_knn.loc[:,feature_selector.get_support()]

#understand how much better it is to have 2 vars than any other #

plt.plot(range(1,len(fit.grid_scores_)+1),fit.grid_scores_,marker='o')
#range used so x axis goes from 1 to 4 features rather than starting at 0
#second fit is the scores for each number of vars
plt.ylabel("model score")
plt.xlabel('number of features')
plt.title(f'Feature Selection using RFE \n Optimal number of features is {optimal_feature_count} (at score of {round(max(fit.grid_scores_),3)})')
plt.tight_layout()
plt.show()
```
<img src="/img/KNN_Features.png">

```python
clf=KNeighborsClassifier()
clf.fit(X_train_knn,y_train)
```

```python
y_pred_class = clf.predict(X_test_knn)
y_pred_proba = clf.predict_proba(X_test_knn)[:,1]


confusion_matrix(y_test, y_pred_class)

plot_confusion_matrix(clf,X_test_knn,y_test,cmap='coolwarm')

#Accuracy ((# of correct classifactions out of all attempted classifications))

accuracy_score(y_test,y_pred_class)

# Precision (of all observations that were predicted +, how many were actually +)
precision_score(y_test,y_pred_class)

#Recall (of all + observations, how many did we predict as +)
recall_score(y_test,y_pred_class)

#f1 score (harmonic mean of recall and precision)
f1_score(y_test,y_pred_class)
```

<img src="/img/KNN_F1.png">

```python
k_list = list(range(2,25))

accuracy_scores= []
for k in k_list:
    clf= KNeighborsClassifier(n_neighbors=k)
    clf.fit(X_train_knn,y_train)
    y_pred=clf.predict(X_test_knn)
    accuracy = f1_score(y_test,y_pred)
    accuracy_scores.append(accuracy)

max_accuracy = max(accuracy_scores)
max_accuracy_idx = accuracy_scores.index(max_accuracy)
optimal_k_value = k_list[max_accuracy_idx]

#plot of max depths

plt.plot(k_list,accuracy_scores)
plt.scatter(optimal_k_value,max_accuracy,marker='x',color='red')
plt.title(f"Accuracy (F1 Score) by K \n Optimal Value for K: {optimal_k_value} (Accuracy: {round(max_accuracy,4)})")
plt.xlabel('k')
plt.ylabel('accuracy (F1 Score)')
plt.tight_layout()
plt.show()
```


### Random Forest Classification Analysis

```python
clf=RandomForestClassifier(random_state=42,n_estimators=500,max_features=5)
clf.fit(X_train,y_train)
```

```python
y_pred_class = clf.predict(X_test)
y_pred_proba = clf.predict_proba(X_test)[:,1]


confusion_matrix(y_test, y_pred_class)

plot_confusion_matrix(clf,X_test,y_test,cmap='coolwarm')
```

<img src="/img/RF_Confusion_Matrix.png">


```python
accuracy_score(y_test,y_pred_class)
```

```python
precision_score(y_test,y_pred_class)
```

```python
recall_score(y_test,y_pred_class)
```

```python
f1_score(y_test,y_pred_class)
```


```python
feature_importance=pd.DataFrame(clf.feature_importances_)
feature_names=pd.DataFrame(X.columns)
feature_importance_summary = pd.concat([feature_names,feature_importance],axis=1)
feature_importance_summary.columns = ['input_variable','feature_importance']
feature_importance_summary.sort_values(by='feature_importance', inplace=True)

plt.barh(feature_importance_summary['input_variable'],feature_importance_summary['feature_importance'])
plt.title("Feature Importance of Random Forests")
plt.xlabel("Feature Importance")
plt.tight_layout()
plt.show()
```
<img src="/img/Feature_Importance1.png">

```python
result = permutation_importance(clf,X_test, y_test, n_repeats=10,random_state=42)
print(result)


permutation_importance=pd.DataFrame(result['importances_mean'])
feature_names=pd.DataFrame(X.columns)
permutation_importance_summary = pd.concat([feature_names,permutation_importance],axis=1)
permutation_importance_summary.columns = ['input_variable','permutation_importance']
permutation_importance_summary.sort_values(by='permutation_importance', inplace=True)
```

```python
plt.barh(permutation_importance_summary['input_variable'],permutation_importance_summary['permutation_importance'])
plt.title("Permutation Importance of Random Forests")
plt.xlabel("Permutation Importance")
plt.tight_layout()
plt.show()
```
<img src="/img/Permutation_Importance1.png">


### Conclusions and Future Work



<!-- <iframe width="1140" height="541.25" src="https://app.powerbi.com/reportEmbed?reportId=d678e41c-01d8-4402-8405-23b635476160&autoAuth=true&ctid=5ebbcbe0-fcd6-4376-b0ed-e060d29cb79e&config=eyJjbHVzdGVyVXJsIjoiaHR0cHM6Ly93YWJpLXVzLW5vcnRoLWNlbnRyYWwtcmVkaXJlY3QuYW5hbHlzaXMud2luZG93cy5uZXQvIn0%3D" frameborder="0" allowFullScreen="true"></iframe>
 -->