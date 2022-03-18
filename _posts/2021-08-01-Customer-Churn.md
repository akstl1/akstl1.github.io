---
layout: post
title: Telecom Customer Churn Prediction
image: "/posts/vehicle_emissions.jpg"
tags: [Churn Analysis, Classification, Random Forest, KNN, Logistic Regression]
---

# Supervised Learning Capstone Project

In this notebook, telecom customer data was read in to determine whether customer churn can be predicted. As shown below, both random forest and logistic regression modelling yielded similar results with accuracies of ~80% on the test set data.

One key insight from the data was also that customers with month-to-month contracts are more likely to churn than other customers. In this subset of customers, the shorter tenure a customer has the higher they are to churn.

<i>Please note that this project was done as part of the 2021 Python for Machine Learning & Data Science Masterclass on Udemy</i>

### Summary of Results

Based on the models run, customer churn can be predicted with ~79% accuracy via a random forest or logistic regression model.

From our EDA, it appears that contract type in particular can be important in predicting churn. Specifically, customers who are on a month to month plan are more likely to churn than other contract types, and especially those who have had plans for 0-12 months.

## Import required packages

``` python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

# EDA

### Load Data

First I load in the relevant data.

``` python
df = pd.read_csv('./DATA/Telco-Customer-Churn.csv')
```
