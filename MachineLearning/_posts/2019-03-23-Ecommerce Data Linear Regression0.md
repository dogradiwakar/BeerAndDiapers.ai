---
layout: post
title: Linear Regression Model on Ecommerce data
description: >

image:
noindex: true
categories: [MachineLearning]
---


```python
#import necessary libraries
import pandas as pd
import numpy as np
%matplotlib inline
import seaborn as sns
import matplotlib.pyplot as plt
```


```python
#load csv
df=pd.read_csv('Ecommerce Customers.csv')
```


```python
#Check Data
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Email</th>
      <th>Address</th>
      <th>Avatar</th>
      <th>Avg. Session Length</th>
      <th>Time on App</th>
      <th>Time on Website</th>
      <th>Length of Membership</th>
      <th>Yearly Amount Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>mstephenson@fernandez.com</td>
      <td>835 Frank Tunnel\nWrightmouth, MI 82180-9605</td>
      <td>Violet</td>
      <td>34.497268</td>
      <td>12.655651</td>
      <td>39.577668</td>
      <td>4.082621</td>
      <td>587.951054</td>
    </tr>
    <tr>
      <th>1</th>
      <td>hduke@hotmail.com</td>
      <td>4547 Archer Common\nDiazchester, CA 06566-8576</td>
      <td>DarkGreen</td>
      <td>31.926272</td>
      <td>11.109461</td>
      <td>37.268959</td>
      <td>2.664034</td>
      <td>392.204933</td>
    </tr>
    <tr>
      <th>2</th>
      <td>pallen@yahoo.com</td>
      <td>24645 Valerie Unions Suite 582\nCobbborough, D...</td>
      <td>Bisque</td>
      <td>33.000915</td>
      <td>11.330278</td>
      <td>37.110597</td>
      <td>4.104543</td>
      <td>487.547505</td>
    </tr>
    <tr>
      <th>3</th>
      <td>riverarebecca@gmail.com</td>
      <td>1414 David Throughway\nPort Jason, OH 22070-1220</td>
      <td>SaddleBrown</td>
      <td>34.305557</td>
      <td>13.717514</td>
      <td>36.721283</td>
      <td>3.120179</td>
      <td>581.852344</td>
    </tr>
    <tr>
      <th>4</th>
      <td>mstephens@davidson-herman.com</td>
      <td>14023 Rodriguez Passage\nPort Jacobville, PR 3...</td>
      <td>MediumAquaMarine</td>
      <td>33.330673</td>
      <td>12.795189</td>
      <td>37.536653</td>
      <td>4.446308</td>
      <td>599.406092</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Get basic details about the dataset
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 500 entries, 0 to 499
    Data columns (total 8 columns):
    Email                   500 non-null object
    Address                 500 non-null object
    Avatar                  500 non-null object
    Avg. Session Length     500 non-null float64
    Time on App             500 non-null float64
    Time on Website         500 non-null float64
    Length of Membership    500 non-null float64
    Yearly Amount Spent     500 non-null float64
    dtypes: float64(5), object(3)
    memory usage: 31.3+ KB



```python
#Get statistical information about the data
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Avg. Session Length</th>
      <th>Time on App</th>
      <th>Time on Website</th>
      <th>Length of Membership</th>
      <th>Yearly Amount Spent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>500.000000</td>
      <td>500.000000</td>
      <td>500.000000</td>
      <td>500.000000</td>
      <td>500.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>33.053194</td>
      <td>12.052488</td>
      <td>37.060445</td>
      <td>3.533462</td>
      <td>499.314038</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.992563</td>
      <td>0.994216</td>
      <td>1.010489</td>
      <td>0.999278</td>
      <td>79.314782</td>
    </tr>
    <tr>
      <th>min</th>
      <td>29.532429</td>
      <td>8.508152</td>
      <td>33.913847</td>
      <td>0.269901</td>
      <td>256.670582</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>32.341822</td>
      <td>11.388153</td>
      <td>36.349257</td>
      <td>2.930450</td>
      <td>445.038277</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>33.082008</td>
      <td>11.983231</td>
      <td>37.069367</td>
      <td>3.533975</td>
      <td>498.887875</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>33.711985</td>
      <td>12.753850</td>
      <td>37.716432</td>
      <td>4.126502</td>
      <td>549.313828</td>
    </tr>
    <tr>
      <th>max</th>
      <td>36.139662</td>
      <td>15.126994</td>
      <td>40.005182</td>
      <td>6.922689</td>
      <td>765.518462</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Get Column names
df.columns
```




    Index(['Email', 'Address', 'Avatar', 'Avg. Session Length', 'Time on App',
           'Time on Website', 'Length of Membership', 'Yearly Amount Spent'],
          dtype='object')



## Exploratory Data Analysis


```python
#Use Seaborn to create a joint plot to find corelation between time on website with yearly amount column
sns.jointplot(x='Time on Website',y='Yearly Amount Spent', data=df)
#doesnt show much of a trend between the two columns
```




    <seaborn.axisgrid.JointGrid at 0x1e64fb66470>




![png](/Tathastu/images/EcommerceDataLinearRegression/output_7_1.png)



```python
#Use Seaborn to create a joint plot to find corelation between time on App with yearly amount column
sns.jointplot(x='Time on App',y='Yearly Amount Spent', data=df)
# shows some corelation between 2 columns
```




    <seaborn.axisgrid.JointGrid at 0x1e64fc42ba8>




![png](output_8_1.png)



```python
#Plot a Hex plot to find more corelationship between time on app and yearly amount
sns.jointplot(x='Time on App',y='Length of Membership', data=df,kind='hex')
```




    <seaborn.axisgrid.JointGrid at 0x1e64fd386a0>




![png](output_9_1.png)



```python
#Create a pairplot to find corelationship between all the columns
sns.pairplot(df)
#seems that the yealy amount spent has the most corelation with the years of membership  
```




    <seaborn.axisgrid.PairGrid at 0x1e64fe085c0>




![png](output_10_1.png)



```python
sns.distplot(df['Yearly Amount Spent'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1e6506b9710>




![png](output_11_1.png)



```python
#Create a linear plot also the error bars are really less which kind of shows that length of membership is directly propotional to the yealy amount spent
sns.lmplot(x='Length of Membership',y='Yearly Amount Spent',data=df)
```




    <seaborn.axisgrid.FacetGrid at 0x1e65096da90>




![png](output_12_1.png)



```python
sns.heatmap(df.corr())
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1e64fc19208>




![png](output_13_1.png)


# Training & Testing Data


```python
#mention all numeric columns
X=df[['Avg. Session Length','Time on App','Time on Website','Length of Membership']]
```


```python
#mention the columns which needs to be predicted
y=df[['Yearly Amount Spent']]
```


```python
#import the train test split
from sklearn.model_selection import train_test_split
```


```python
#Tuple Unpacking
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=101)
```

### Train the Model


```python
from sklearn.linear_model import LinearRegression
```


```python
#Create an instance of LinearRegression() Model
lm = LinearRegression()
```


```python
#Fit Data on lm
lm.fit(X_train,y_train)
```




    LinearRegression(copy_X=True, fit_intercept=True, n_jobs=None, normalize=False)




```python
#print out the intercepts
print (lm.intercept_)
```

    [-1047.93278225]



```python
#print coefficients
lm.coef_
```




    array([[25.98154972, 38.59015875,  0.19040528, 61.27909654]])



### Predicting test Data


```python
#predict x_test data
prediction=lm.predict(X_test)
```


```python
#compare real vs predicted values
plt.scatter(y_test, prediction)
plt.xlabel('true values')
plt.ylabel('predicted values')
```




    Text(0, 0.5, 'predicted values')




![png](output_27_1.png)


### Evaluating the Model


```python
from sklearn import metrics
```


```python
#mean_absolute_error
metrics.mean_absolute_error(y_test,prediction)
```




    7.22814865343082




```python
#mean_squared_error
metrics.mean_squared_error(y_test,prediction)
```




    79.81305165097419




```python
#Root mean_absolute_error
np.sqrt(metrics.mean_squared_error(y_test,prediction))
```




    8.93381506697862




```python
#Variance in Model How well does it fit
metrics.explained_variance_score(y_test,prediction)
```




    0.9890771231889607




```python
# Plot on residuals
sns.distplot(y_test- prediction, bins=50)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1e652607128>




![png](output_34_1.png)



```python
cdf = pd.DataFrame(lm.coef_.transpose(),X.columns, columns=['coeff'])
cdf

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>coeff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Avg. Session Length</th>
      <td>25.981550</td>
    </tr>
    <tr>
      <th>Time on App</th>
      <td>38.590159</td>
    </tr>
    <tr>
      <th>Time on Website</th>
      <td>0.190405</td>
    </tr>
    <tr>
      <th>Length of Membership</th>
      <td>61.279097</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
