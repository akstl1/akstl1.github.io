---
layout: post
title: Heart Disease Classification
image: "/posts/heart_disease_1.jpg"
tags: [Python, Classification, Random Forest, KNN, Logistic Classification]
---

# Part 1 - INTRODUCTION
Heart disease describes a variety of related conditions such as Coronary Artery Disease, Acute coronary syndrome, Angina, and Aortic Anuerism. Heart disease can lead to numerous detrimental or fatal conditions such as diabetes, heart failure, and heart attack.

It is the leading cause of death in the Unites States, according to the CDC. Roughly 660,000 - 700,00 people in the US die each year from heart disease, which is about one-quarter of all yearly deaths. Approximately 18 million people wordwide die due to heart disease annually, which is about 32% of all deaths.

Given the disease's prevelence and series symptoms, doctors are collecting data from patients to assess a patient's risk for heart disease and prevent it if possible. In this analysis, I will use a set of patient data and use it to predict a patient's risk for having heart disease.

To do this, I will clean data and then utilize different machine learning models to predict a patient's risk of having heart disease. I will then take the most successful model and deploy it so others could, in theory, use it to predict this risk in real time.

## ---- Results ----
After running several different models on the data, I determined that a random forest model was best able to predict heart disease risk with an accuracy of 96.6% and F1 score of 96.8%. With this finding, I then deployed the model to Heroku so others could (theoretically) use it for prediction purposes.

## ---- Data Source and Notes ----
Data source: https://www.kaggle.com/johnsmith88/heart-disease-dataset/version/2

Data column dictionary:

1. age
2. sex
3. chest pain type (4 values)
4. resting blood pressure
5. serum cholestoral in mg/dl
6. fasting blood sugar > 120 mg/dl
7. resting electrocardiographic results (values 0,1,2)
8. maximum heart rate achieved
9. exercise induced angina
10. oldpeak = ST depression induced by exercise relative to rest
11. the slope of the peak exercise ST segment
12. number of major vessels (0-3) colored by flourosopy
13. thal: 0 = normal; 1 = fixed defect; 2 = reversable defect

## ---- Import Libraries for Project ----
```python
#analysis and viz imports
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

#data preparation imports
from sklearn.utils import shuffle
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import OneHotEncoder
from sklearn.feature_selection import RFECV
from sklearn.preprocessing import OneHotEncoder, MinMaxScaler

#ML model imports
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.neighbors import KNeighborsClassifier

#metric and analysis imports
from sklearn.metrics import confusion_matrix,plot_confusion_matrix,accuracy_score,precision_score,recall_score,f1_score
from sklearn.inspection import permutation_importance

#deployment imports
import joblib
```

# Part 2 - Data Preparation And Exploration
## ---- Load Data ----

```python
heart_disease_data = pd.read_csv("./heart.csv")

#use the head method to get a glimpse of the data structure before moving forward
heart_disease_data.head()
```

|age|	sex|	cp|	trestbps|	chol|	fbs|	restecg|	thalach|	exang|	oldpeak|	slope|	ca|	thal|	target|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|527|	62|	0|	0|	124|	209|	0|	1|	163|	0|	0.0|	2|	0|	2|	1|
|359|	53|	0|	2|	128|	216|	0|	0|	115|	0|	0.0|	2|	0|	0|	1|
|447|	55|	1|	0|	160|	289|	0|	0|	145|	1|	0.8|	1|	1|	3|	0|
|31|	50|	0|	1|	120|	244|	0|	1|	162|	0|	1.1|	2|	0|	2|	1|
|621|	48|	1|	0|	130|	256|	1|	0|	150|	1|	0.0|	2|	2|	3|	0|

## ---- Clean Data ----
First, I will check whether there are any missing values in any of the data rows.

```python
heart_disease_data.info()
```

|# |  Column|    Non-Null| Count|  Dtype|
|---|---|---|---|---|
 |0|   age||       1025 non-null|   int64|  
 |1|   sex |      1025 non-null|   int64 | 
 |2|   cp  |      1025 non-null|   int64 | 
 |3|   trestbps|  1025 non-null|   int64 | 
 |4|   chol|      1025 non-null|   int64 | 
 |5|   fbs|       1025 non-null|   int64 | 
 |6|   restecg|   1025 non-null|   int64 | 
 |7|   thalach|   1025 non-null|   int64 | 
 |8|   exang |    1025 non-null|   int64 | 
 |9|   oldpeak |  1025 non-null|   float64|
 |10|  slope |    1025 non-null|   int64 | 
 |11|  ca   |     1025 non-null|   int64 | 
 |12|  thal   |   1025 non-null|   int64 | 
 |13|  target |   1025 non-null|   int64 | 

The above table shows that there are no null values in the dataset, so nulls do not have to be removed/infilled.

Next I will view the description of the dataset. In the data description, the data uploader indicates that certain variables are boolean, some range for 0-3, and others like age have some common limits like age not being negative. I will use this data and check data ranges to make sure data is within proper bounds.

```python
heart_disease_data.describe()
```
| |age|	sex|	cp|	trestbps|	chol|	fbs|	restecg|	thalach|	exang|	oldpeak|	slope|	ca|	thal|	target|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|mean|	54.434146|	0.695610|	0.942439|	131.611707|	246.00000|	0.149268|	0.529756|	149.114146|	0.336585|	1.071512|	1.385366|	0.754146|	2.323902|	0.513171|
|std|	9.072290|	0.460373|	1.029641|	17.516718|	51.59251|	0.356527|	0.527878|	23.005724|	0.472772|	1.175053|	0.617755|	1.030798|	0.620660|	0.500070|
|min|	29.000000|	0.000000|	0.000000|	94.000000|	126.00000|	0.000000|	0.000000|	71.000000|	0.000000|	0.000000|	0.000000|	0.000000|	0.000000|	0.000000|
|25%|	48.000000|	0.000000|	0.000000|	120.000000|	211.00000|	0.000000|	0.000000|	132.000000|	0.000000|	0.000000|	1.000000|	0.000000|	2.000000|	0.000000|
|50%|	56.000000|	1.000000|	1.000000|	130.000000|	240.00000|	0.000000|	1.000000|	152.000000|	0.000000|	0.800000|	1.000000|	0.000000|	2.000000|	1.000000|
|75%|	61.000000|	1.000000|	2.000000|	140.000000|	275.00000|	0.000000|	1.000000|	166.000000|	1.000000|	1.800000|	2.000000|	1.000000|	3.000000|	1.000000|
|max|	77.000000|	1.000000|	3.000000|	200.000000|	564.00000|	1.000000|	2.000000|	202.000000|	1.000000|	6.200000|	2.000000|	4.000000|	3.000000|	1.000000|

```python
heart_disease_data["cp"].unique()
#array([0, 2, 1, 3], dtype=int64)

heart_disease_data["fbs"].unique()
#array([0, 1], dtype=int64)

heart_disease_data["restecg"].unique()
#array([1, 0, 2], dtype=int64)

heart_disease_data["exang"].unique()
#array([0, 1], dtype=int64)

heart_disease_data["slope"].unique()
#array([2, 1, 0], dtype=int64)

heart_disease_data["ca"].unique()
#array([0, 1, 2, 4, 3], dtype=int64)

heart_disease_data["thal"].unique()
#array([2, 0, 3, 1], dtype=int64)

heart_disease_data["target"].unique()
#array([1, 0], dtype=int64)
```
Looking at the data, all columns have data within reasonable limits and no corrections need to be made.

Next I will shuffle the data, according to the target variable, to help randomize distribution into train and test data sets.

```python
#shuffle data, to ensure that target variable rows are randomly dispersed. This may help when splitting into 
#test and train sets

heart_disease_data = shuffle(heart_disease_data,random_state=42)
```
I will also check the distribution of the target variable in the data. If data is imbalanced, we may need to take additional steps in our analysis to account for this.

```python
#Check the distribution of the target variable in this data. If the target is imbalanced, additional steps may 
#need to be taken accordingly

heart_disease_data['target'].value_counts(normalize=True)

#1    0.513171
#0    0.486829
```

According to the above calculation, the target variable distribution in this data is roughly a 50/50 split between the 0 and 1 value. Since there isn't a clear imbalance, we can proceed without additional measures.

## ---- Explore the data (EDA) ----
In this classification task, I want to search for correlations between variables and especially with the target variable. To do so I will use a heatmap and correlation matrix.

```python
sns.heatmap(heart_disease_data.corr());
```
<a href="./img/posts/heart_disease_2.png"></a>

In the correlation matrix / heat map, I see that there may be correlations bewteen the target variable and the age, sex, exang, oldpeak, ca and thal variables. At this point I cannot discount any variables yet, but it is helpful to keep these in mind going forward.

## ---- Hypothesize a Solution ----
In the next parts of this analysis, I will attempt to classify the given data points and predict whether a given patient has heart disease. To do so, I will test three models:

1. Random Forest Classifier
2. KNN Classifier
3. Logistic Regression Classifier

# Part 3 Develop a Model
## ---- Split Data For Models ----

```python
X=heart_disease_data.drop('target',axis=1)
y=heart_disease_data['target']

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42,stratify=y)
```
