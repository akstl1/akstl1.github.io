---
layout: post
title: Parkinson's Detection
image: "/posts/parkinsons_image.jpg"
tags: [Python, Parkinson's, Classification]
---

# Insert Intro Here

I love Python & Coffee so much, here is that picture from the top of my project AGAIN, but this time, in the BODY of my project!

<!-- ![alt text](/img/posts/coffee_python.jpg "Coffee & Python - I love them!") -->


```
my_love_for_python = 0
my_python_knowledge = 0

for day in lifetime:
    my_love_for_python += 1
    my_python_knowledge += 1
```


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

```python
data = pd.read_csv('../input/parkinsons-data-set/parkinsons.data')

```

### Perform EDA


```python
data.head()
```

```python
data.drop('name',axis=1,inplace=True)
```

```python
data.info()

```



```python
data = shuffle(data,random_state=42)
```

```python
data['status'].value_counts(normalize=True) 
```

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

#Accuracy ((# of correct classifactions out of all attempted classifications))

accuracy_score(y_test,y_pred_class)

# Precision (of all observations that were predicted +, how many were actually +)
precision_score(y_test,y_pred_class)

#Recall (of all + observations, how many did we predict as +)
recall_score(y_test,y_pred_class)

#f1 score (harmonic mean of recall and precision)
f1_score(y_test,y_pred_class)
```










<!-- <iframe width="1140" height="541.25" src="https://app.powerbi.com/reportEmbed?reportId=d678e41c-01d8-4402-8405-23b635476160&autoAuth=true&ctid=5ebbcbe0-fcd6-4376-b0ed-e060d29cb79e&config=eyJjbHVzdGVyVXJsIjoiaHR0cHM6Ly93YWJpLXVzLW5vcnRoLWNlbnRyYWwtcmVkaXJlY3QuYW5hbHlzaXMud2luZG93cy5uZXQvIn0%3D" frameborder="0" allowFullScreen="true"></iframe>
 -->
