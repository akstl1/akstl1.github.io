---
layout: post
title: Parkinson's Detection
image: "/posts/parkinsons_image.jpg"
tags: [Python, Parkinson's, Classification]
---

# Insert Intro Here

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







<!-- Just so you really see how much I love Python, here is some code BUT with some colours for keywords & functionality!

```python
my_love_for_python = 0
my_python_knowledge = 0

for day in lifetime:
    my_love_for_python += 1
    my_python_knowledge += 1  
```

Here is an **unordered list** showing some things I love about Python

* For my work
    * Data Analysis
    * Data Visualisation
    * Machine Learning
* For fun
    * Deep Learning
    * Computer Vision
    * Projects about coffee

Here is an _ordered list_ showing some things I love about coffee

1. The smell
    1. Especially in the morning, but also at all times of the day!
2. The taste
3. The fact I can run the 100m in approx. 9 seconds after having 4 cups in quick succession

I love Python & Coffee so much, here is that picture from the top of my project AGAIN, but this time, in the BODY of my project!

![alt text](/img/posts/coffee_python.jpg "Coffee & Python - I love them!")

The above image is just linked to the actual file in my Github, but I could also link to images online, using the URL!

A line break, like this one below - helps me make sense of what I'm reading, especially when I've had so much coffee that my vision goes a little blurry

---

I could also add things to my project like links, tables, quotes, and HTML blocks - but I'm starting to get a cracking headache.  Must be coffee time.
 -->
<!-- <iframe width="1140" height="541.25" src="https://app.powerbi.com/reportEmbed?reportId=d678e41c-01d8-4402-8405-23b635476160&autoAuth=true&ctid=5ebbcbe0-fcd6-4376-b0ed-e060d29cb79e&config=eyJjbHVzdGVyVXJsIjoiaHR0cHM6Ly93YWJpLXVzLW5vcnRoLWNlbnRyYWwtcmVkaXJlY3QuYW5hbHlzaXMud2luZG93cy5uZXQvIn0%3D" frameborder="0" allowFullScreen="true"></iframe>
 -->
