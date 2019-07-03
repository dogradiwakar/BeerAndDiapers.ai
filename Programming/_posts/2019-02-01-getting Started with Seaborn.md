---
layout: post
title: Getting Started with Seaborn
description: >

author: author1
canonical_url:
categories: [Programming,Seaborn]
#tags: [Programming1 , Numpy1]
---
### Getting Started with Seaborn

# Distribution Plots

* distplot
* jointplot
* pairplot
* rugplot
* kdeplot


```python
import seaborn as sns
# To show the graphs within the notebook
%matplotlib inline
```


```python
tips=sns.load_dataset('tips')
tips.head()
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
      <th>total_bill</th>
      <th>tip</th>
      <th>sex</th>
      <th>smoker</th>
      <th>day</th>
      <th>time</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16.99</td>
      <td>1.01</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.34</td>
      <td>1.66</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.01</td>
      <td>3.50</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>23.68</td>
      <td>3.31</td>
      <td>Male</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24.59</td>
      <td>3.61</td>
      <td>Female</td>
      <td>No</td>
      <td>Sun</td>
      <td>Dinner</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



# DistPlot
The distplot shows the distribution of a univariate set of observations.


```python

sns.distplot(tips['total_bill'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2058a0d8198>



![png](/BeerAndDiapers.ai/images/Seaborn/output_4_1.png)



```python
# To remove the KDE set kde parameter to false and to set bins set value of bins accordingly
sns.distplot(tips['total_bill'],kde=False,bins=20)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2058a3c1ba8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_5_1.png)


# Jointplot
jointplot() allows you to basically match up two distplots for bivariate data


```python
# Various kind paramaters scatter , reg, resid, kde, hex
sns.jointplot(x='total_bill',y='tip',data=tips, kind='scatter')
sns.jointplot(x='total_bill',y='tip',data=tips, kind='kde')
sns.jointplot(x='total_bill',y='tip',data=tips, kind='hex')
sns.jointplot(x='total_bill',y='tip',data=tips, kind='reg')
```




    <seaborn.axisgrid.JointGrid at 0x2058bd6f390>




![png](/BeerAndDiapers.ai/images/Seaborn/output_7_1.png)



![png](/BeerAndDiapers.ai/images/Seaborn/output_7_2.png)



![png](/BeerAndDiapers.ai/images/Seaborn/output_7_3.png)



![png](/BeerAndDiapers.ai/images/Seaborn/output_7_4.png)


# pairplot
pairplot will plot pairwise relationships across an entire dataframe (for all the numerical columns) and supports a color hue argument (for categorical columns)


```python
sns.pairplot(tips)
```




    <seaborn.axisgrid.PairGrid at 0x2058d3584e0>




![png](/BeerAndDiapers.ai/images/Seaborn/output_9_1.png)



```python
sns.pairplot(tips, hue='sex',palette='coolwarm')
```




    <seaborn.axisgrid.PairGrid at 0x2058e176668>




![png](/BeerAndDiapers.ai/images/Seaborn/output_10_1.png)


# rugplot
rugplots just draw a dash mark for every point on a univariate distribution.


```python
sns.rugplot(tips['total_bill'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2058eba3470>




![png](/BeerAndDiapers.ai/images/Seaborn/output_12_1.png)


# kdeplot
kdeplots are Kernel Density Estimation plots. These KDE plots replace every single observation with a Gaussian (Normal) distribution centered around that value


```python
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats

#Create dataset
dataset = np.random.randn(25)

# Create another rugplot
sns.rugplot(dataset);

# Set up the x-axis for the plot
x_min = dataset.min() - 2
x_max = dataset.max() + 2

# 100 equally spaced points from x_min to x_max
x_axis = np.linspace(x_min,x_max,100)

# Set up the bandwidth, for info on this:
url = 'http://en.wikipedia.org/wiki/Kernel_density_estimation#Practical_estimation_of_the_bandwidth'

bandwidth = ((4*dataset.std()**5)/(3*len(dataset)))**.2


# Create an empty kernel list
kernel_list = []

# Plot each basis function
for data_point in dataset:

    # Create a kernel for each point and append to list
    kernel = stats.norm(data_point,bandwidth).pdf(x_axis)
    kernel_list.append(kernel)

    #Scale for plotting
    kernel = kernel / kernel.max()
    kernel = kernel * .4
    plt.plot(x_axis,kernel,color = 'grey',alpha=0.5)

plt.ylim(0,1)
```




    (0, 1)




![png](/BeerAndDiapers.ai/images/Seaborn/output_14_1.png)



```python
# To get the kde plot we can sum these basis functions.

# Plot the sum of the basis function
sum_of_kde = np.sum(kernel_list,axis=0)

# Plot figure
fig = plt.plot(x_axis,sum_of_kde,color='indianred')

# Add the initial rugplot
sns.rugplot(dataset,c = 'indianred')

# Get rid of y-tick marks
plt.yticks([])

# Set title
plt.suptitle("Sum of the Basis Functions")
```




    Text(0.5, 0.98, 'Sum of the Basis Functions')




![png](/BeerAndDiapers.ai/images/Seaborn/output_15_1.png)



```python
sns.kdeplot(tips['total_bill'])
sns.rugplot(tips['total_bill'])
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2058fe52780>




![png](/BeerAndDiapers.ai/images/Seaborn/output_16_1.png)


# Categorical Data Plots

* factorplot
* boxplot
* violinplot
* stripplot
* swarmplot
* barplot
* countplot


# barplot and countplot

These plots allow to get aggregate data off a categorical feature in your data. **barplot** is a general plot that allows you to aggregate the categorical data based off some function, by default the mean. **Count plot** does the aggregation at counts


```python
sns.barplot(x='sex',y='total_bill',data=tips)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20591a0fc18>




![png](/BeerAndDiapers.ai/images/Seaborn/output_19_1.png)



```python
#using estimator we can override default aggregation type
import numpy as np
sns.barplot(x='sex',y='total_bill',data=tips,estimator=np.sum)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x205907a01d0>




![png](/BeerAndDiapers.ai/images/Seaborn/output_20_1.png)



```python
sns.countplot(x='sex',data=tips)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20591084828>




![png](/BeerAndDiapers.ai/images/Seaborn/output_21_1.png)


# boxplot and violinplot

boxplots and violinplots are used to shown the distribution of categorical data.

A **box plot** shows the distribution of quantitative data in a way that facilitates comparisons between variables or across levels of a categorical variable. The box shows the quartiles of the dataset while the whiskers extend to show the rest of the distribution, except for points that are determined to be “outliers” using a method that is a function of the inter-quartile range.


A **violin plot** plays a similar role as a box and whisker plot. It shows the distribution of quantitative data across several levels of one (or more) categorical variables such that those distributions can be compared. Unlike a box plot, in which all of the plot components correspond to actual datapoints, the violin plot features a kernel density estimation of the underlying distribution.


```python
sns.boxplot(x='day',y='total_bill',data=tips, palette='rainbow')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20591c8eba8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_23_1.png)



```python
#To do the boxplot on entiredataframe
sns.boxplot(data=tips, palette='rainbow',orient='h')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20591bacb00>




![png](/BeerAndDiapers.ai/images/Seaborn/output_24_1.png)



```python
# to add another categor add hue
sns.boxplot(x='day',y='total_bill',data=tips, palette='rainbow',hue='sex')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20591e005f8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_25_1.png)



```python
sns.violinplot(x='day',y='total_bill',data=tips, palette='rainbow')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20592f2b8d0>




![png](/BeerAndDiapers.ai/images/Seaborn/output_26_1.png)



```python
sns.violinplot(x='day',y='total_bill',data=tips, palette='rainbow',hue='sex')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2059302ec18>




![png](/BeerAndDiapers.ai/images/Seaborn/output_27_1.png)



```python
#use split to merge into 1
sns.violinplot(x="day", y="total_bill", data=tips,hue='sex',split=True,platette='set1')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20593306198>




![png](/BeerAndDiapers.ai/images/Seaborn/output_28_1.png)


# stripplot and swarmplot

The stripplot will draw a scatterplot where one variable is categorical. A strip plot can be drawn on its own, but it is also a good complement to a box or violin plot in cases where you want to show all observations along with some representation of the underlying distribution.

The swarmplot is similar to stripplot(), but the points are adjusted (only along the categorical axis) so that they don’t overlap. This gives a better representation of the distribution of values, although it does not scale as well to large numbers of observations (both in terms of the ability to show all the points and in terms of the computation needed to arrange them).


```python
sns.stripplot(x="day", y="total_bill", data=tips)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20593382cc0>




![png](/BeerAndDiapers.ai/images/Seaborn/output_30_1.png)



```python
sns.stripplot(x="day", y="total_bill", data=tips,jitter=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x205933e3e10>




![png](/BeerAndDiapers.ai/images/Seaborn/output_31_1.png)



```python
sns.stripplot(x="day", y="total_bill", data=tips,jitter=True,hue='sex',palette='Set1')
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20593413a58>




![png](/BeerAndDiapers.ai/images/Seaborn/output_32_1.png)



```python
sns.stripplot(x="day", y="total_bill", data=tips,jitter=True,hue='sex',palette='Set1',split=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20593623ac8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_33_1.png)



```python
sns.swarmplot(x="day", y="total_bill", data=tips)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2059368ce10>




![png](/BeerAndDiapers.ai/images/Seaborn/output_34_1.png)



```python
sns.swarmplot(x="day", y="total_bill",hue='sex',data=tips, palette="Set1", split=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x2059373d860>




![png](/BeerAndDiapers.ai/images/Seaborn/output_35_1.png)



```python
#Combining Categorical Plots
sns.violinplot(x="tip", y="day", data=tips,palette='rainbow')
sns.swarmplot(x="tip", y="day", data=tips,color='black',size=3)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20594785ef0>




![png](/BeerAndDiapers.ai/images/Seaborn/output_36_1.png)


# factorplot
factorplot is the most general form of a categorical plot. It can take in a kind parameter to adjust the plot type:


```python
sns.factorplot(x='sex',y='total_bill',data=tips,kind='bar')
sns.factorplot(x='sex',y='total_bill',data=tips,kind='box')
sns.factorplot(x='sex',y='total_bill',data=tips,kind='violin')
```




    <seaborn.axisgrid.FacetGrid at 0x20594949828>




![png](/BeerAndDiapers.ai/images/Seaborn/output_38_1.png)



![png](/BeerAndDiapers.ai/images/Seaborn/output_38_2.png)



![png](/BeerAndDiapers.ai/images/Seaborn/output_38_3.png)


# Matrix Plots¶
Matrix plots allow you to plot data as color-encoded matrices and can also be used to indicate clusters within the data


```python
flights = sns.load_dataset('flights')
flights.head()
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
      <th>year</th>
      <th>month</th>
      <th>passengers</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1949</td>
      <td>January</td>
      <td>112</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1949</td>
      <td>February</td>
      <td>118</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1949</td>
      <td>March</td>
      <td>132</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1949</td>
      <td>April</td>
      <td>129</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1949</td>
      <td>May</td>
      <td>121</td>
    </tr>
  </tbody>
</table>
</div>



# Heatmap


```python
# For Heatmap to work we need to convert the data in matrix form using corr fn or pivoting the data
# Matrix form for correlation data
tp=tips.corr()
```


```python
sns.heatmap(tp)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x205916c6400>




![png](/BeerAndDiapers.ai/images/Seaborn/output_43_1.png)



```python
#use annot to show labels
sns.heatmap(tips.corr(),cmap='coolwarm',annot=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20594b43208>




![png](/BeerAndDiapers.ai/images/Seaborn/output_44_1.png)



```python
flights.pivot_table(values='passengers',index='month',columns='year')
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
      <th>year</th>
      <th>1949</th>
      <th>1950</th>
      <th>1951</th>
      <th>1952</th>
      <th>1953</th>
      <th>1954</th>
      <th>1955</th>
      <th>1956</th>
      <th>1957</th>
      <th>1958</th>
      <th>1959</th>
      <th>1960</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>January</th>
      <td>112</td>
      <td>115</td>
      <td>145</td>
      <td>171</td>
      <td>196</td>
      <td>204</td>
      <td>242</td>
      <td>284</td>
      <td>315</td>
      <td>340</td>
      <td>360</td>
      <td>417</td>
    </tr>
    <tr>
      <th>February</th>
      <td>118</td>
      <td>126</td>
      <td>150</td>
      <td>180</td>
      <td>196</td>
      <td>188</td>
      <td>233</td>
      <td>277</td>
      <td>301</td>
      <td>318</td>
      <td>342</td>
      <td>391</td>
    </tr>
    <tr>
      <th>March</th>
      <td>132</td>
      <td>141</td>
      <td>178</td>
      <td>193</td>
      <td>236</td>
      <td>235</td>
      <td>267</td>
      <td>317</td>
      <td>356</td>
      <td>362</td>
      <td>406</td>
      <td>419</td>
    </tr>
    <tr>
      <th>April</th>
      <td>129</td>
      <td>135</td>
      <td>163</td>
      <td>181</td>
      <td>235</td>
      <td>227</td>
      <td>269</td>
      <td>313</td>
      <td>348</td>
      <td>348</td>
      <td>396</td>
      <td>461</td>
    </tr>
    <tr>
      <th>May</th>
      <td>121</td>
      <td>125</td>
      <td>172</td>
      <td>183</td>
      <td>229</td>
      <td>234</td>
      <td>270</td>
      <td>318</td>
      <td>355</td>
      <td>363</td>
      <td>420</td>
      <td>472</td>
    </tr>
    <tr>
      <th>June</th>
      <td>135</td>
      <td>149</td>
      <td>178</td>
      <td>218</td>
      <td>243</td>
      <td>264</td>
      <td>315</td>
      <td>374</td>
      <td>422</td>
      <td>435</td>
      <td>472</td>
      <td>535</td>
    </tr>
    <tr>
      <th>July</th>
      <td>148</td>
      <td>170</td>
      <td>199</td>
      <td>230</td>
      <td>264</td>
      <td>302</td>
      <td>364</td>
      <td>413</td>
      <td>465</td>
      <td>491</td>
      <td>548</td>
      <td>622</td>
    </tr>
    <tr>
      <th>August</th>
      <td>148</td>
      <td>170</td>
      <td>199</td>
      <td>242</td>
      <td>272</td>
      <td>293</td>
      <td>347</td>
      <td>405</td>
      <td>467</td>
      <td>505</td>
      <td>559</td>
      <td>606</td>
    </tr>
    <tr>
      <th>September</th>
      <td>136</td>
      <td>158</td>
      <td>184</td>
      <td>209</td>
      <td>237</td>
      <td>259</td>
      <td>312</td>
      <td>355</td>
      <td>404</td>
      <td>404</td>
      <td>463</td>
      <td>508</td>
    </tr>
    <tr>
      <th>October</th>
      <td>119</td>
      <td>133</td>
      <td>162</td>
      <td>191</td>
      <td>211</td>
      <td>229</td>
      <td>274</td>
      <td>306</td>
      <td>347</td>
      <td>359</td>
      <td>407</td>
      <td>461</td>
    </tr>
    <tr>
      <th>November</th>
      <td>104</td>
      <td>114</td>
      <td>146</td>
      <td>172</td>
      <td>180</td>
      <td>203</td>
      <td>237</td>
      <td>271</td>
      <td>305</td>
      <td>310</td>
      <td>362</td>
      <td>390</td>
    </tr>
    <tr>
      <th>December</th>
      <td>118</td>
      <td>140</td>
      <td>166</td>
      <td>194</td>
      <td>201</td>
      <td>229</td>
      <td>278</td>
      <td>306</td>
      <td>336</td>
      <td>337</td>
      <td>405</td>
      <td>432</td>
    </tr>
  </tbody>
</table>
</div>




```python
pvflights =flights.pivot_table(values='passengers',index='month',columns='year')
sns.heatmap(pvflights)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20594befa20>




![png](/BeerAndDiapers.ai/images/Seaborn/output_46_1.png)



```python
# use line color and line width to improve look and feel
sns.heatmap(pvflights,cmap='magma',linecolor='white',linewidths=1)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x20594de2dd8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_47_1.png)


# clustermap
The clustermap uses hierarchal clustering to produce a clustered version of the heatmap


```python
sns.clustermap(pvflights)
```




    <seaborn.matrix.ClusterGrid at 0x20595e516d8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_49_1.png)



```python
sns.clustermap(pvflights,cmap='coolwarm',standard_scale=1)
```




    <seaborn.matrix.ClusterGrid at 0x20595f35e80>




![png](/BeerAndDiapers.ai/images/Seaborn/output_50_1.png)



# Regression Plots
lmplot allows you to display linear models, but it also conveniently allows you to split up those plots based off of features, as well as coloring the hue based off of features.


```python
sns.lmplot(x='total_bill',y='tip',data=tips)
```




    <seaborn.axisgrid.FacetGrid at 0x20596664208>




![png](/BeerAndDiapers.ai/images/Seaborn/output_52_1.png)



```python
#adding another category
sns.lmplot(x='total_bill',y='tip',data=tips,hue='sex')
```




    <seaborn.axisgrid.FacetGrid at 0x205968690b8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_53_1.png)



```python
sns.lmplot(x='total_bill',y='tip',data=tips,hue='sex',palette='coolwarm')
```




    <seaborn.axisgrid.FacetGrid at 0x205968e1780>




![png](/BeerAndDiapers.ai/images/Seaborn/output_54_1.png)



```python
# specify markers to distinguish ,
sns.lmplot(x='total_bill',y='tip',data=tips,hue='sex',palette='coolwarm',markers=['o','v'],scatter_kws={'s':100})
```




    <seaborn.axisgrid.FacetGrid at 0x205969c03c8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_55_1.png)



```python
## Using a Grid
sns.lmplot(x='total_bill',y='tip',data=tips,col='sex')
```




    <seaborn.axisgrid.FacetGrid at 0x20596ae3c18>




![png](/BeerAndDiapers.ai/images/Seaborn/output_56_1.png)



```python
#provide row and column to lmplot
sns.lmplot(x="total_bill", y="tip", row="sex", col="time",data=tips)
```




    <seaborn.axisgrid.FacetGrid at 0x20597ef0da0>




![png](/BeerAndDiapers.ai/images/Seaborn/output_57_1.png)



```python
# plot for different days
sns.lmplot(x='total_bill',y='tip',data=tips,col='day',hue='sex',palette='coolwarm')
```




    <seaborn.axisgrid.FacetGrid at 0x20598122860>




![png](/BeerAndDiapers.ai/images/Seaborn/output_58_1.png)



```python
#adding aspect and size
sns.lmplot(x='total_bill',y='tip',data=tips,col='day',hue='sex',palette='coolwarm',
          aspect=0.6,size=8)
```




    <seaborn.axisgrid.FacetGrid at 0x20598b19780>




![png](/BeerAndDiapers.ai/images/Seaborn/output_59_1.png)


# Grids¶
Grids are general types of plots that allow you to map plot types to rows and columns of a grid, this helps you create similar plots separated by features.


```python
iris = sns.load_dataset('iris')
iris.head()
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
      <th>sepal_length</th>
      <th>sepal_width</th>
      <th>petal_length</th>
      <th>petal_width</th>
      <th>species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>setosa</td>
    </tr>
  </tbody>
</table>
</div>



# PairGrid
Pairgrid is a subplot grid for plotting pairwise relationships in a dataset.


```python
# Just the Grid
sns.PairGrid(iris)

```




    <seaborn.axisgrid.PairGrid at 0x2059ae0b940>




![png](/BeerAndDiapers.ai/images/Seaborn/output_63_1.png)



```python
# Then you map to the grid
g = sns.PairGrid(iris)
g.map(plt.scatter)
```




    <seaborn.axisgrid.PairGrid at 0x2059be1ac50>




![png](/BeerAndDiapers.ai/images/Seaborn/output_64_1.png)



```python
# Map to upper,lower, and diagonal
g = sns.PairGrid(iris)
g.map_diag(plt.hist)
g.map_upper(plt.scatter)
g.map_lower(sns.kdeplot)
```




    <seaborn.axisgrid.PairGrid at 0x2059d5ff0b8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_65_1.png)


# pairplot
pairplot is a simpler version of PairGrid


```python
sns.pairplot(iris)
```




    <seaborn.axisgrid.PairGrid at 0x20599c9beb8>




![png](/BeerAndDiapers.ai/images/Seaborn/output_67_1.png)



```python
sns.pairplot(iris,hue='species',palette='rainbow')
```




    <seaborn.axisgrid.PairGrid at 0x2059e493b38>




![png](/BeerAndDiapers.ai/images/Seaborn/output_68_1.png)


# Facet Grid¶
FacetGrid is the general way to create grids of plots based off of a feature:


```python
# Just the Grid
g = sns.FacetGrid(tips, col="time", row="smoker")
```


![png](/BeerAndDiapers.ai/images/Seaborn/output_70_0.png)



```python
g = sns.FacetGrid(tips, col="time",  row="smoker")
g = g.map(plt.hist, "total_bill")
```


![png](/BeerAndDiapers.ai/images/Seaborn/output_71_0.png)



```python
g = sns.FacetGrid(tips, col="time",  row="smoker",hue='sex')
# Notice hwo the arguments come after plt.scatter call
g = g.map(plt.scatter, "total_bill", "tip").add_legend()
```


![png](/BeerAndDiapers.ai/images/Seaborn/output_72_0.png)


# JointGrid
JointGrid is the general version for jointplot() type grids, for a quick example:


```python

g = sns.JointGrid(x="total_bill", y="tip", data=tips)
```


![png](/BeerAndDiapers.ai/images/Seaborn/output_74_0.png)



```python
g = sns.JointGrid(x="total_bill", y="tip", data=tips)
g = g.plot(sns.regplot, sns.distplot)
```


![png](/BeerAndDiapers.ai/images/Seaborn/output_75_0.png)



```python
#style and color

sns.set_style('white')
sns.countplot(x='sex',data=tips,palette='deep')
sns.despine()
sns.despine(left=True)
```


![png](/BeerAndDiapers.ai/images/Seaborn/output_76_0.png)
