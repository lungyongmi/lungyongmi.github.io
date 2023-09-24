---
title: "Project 1：Segment Customers with RFM and K-Means"
collection: portfolio
---

### Project Goal: <br/>
<font size=3> Segment customers with RFM and K-Means in order to target customers efficiently. <br/>
透過 RFM 模型與  KMeans 演算法進行客戶分群，找出目標客戶。<br/> </font>

### Dataset Overview: <br/>
<font size=3> This is a transnational data set which contains all the transactions occurring between 2010/12/01 and 2011/12/09 for a UK-based and registered non-store online retail.The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers. </font>

### Data Source: <a href="https://archive.ics.uci.edu/dataset/352/online+retail">UCI Machine Learning Repository</a>
### Tools: Python, Power BI


### Analysis Process:
<font size=3> 
   1. Reading and Exploring Data<br/>
   2. Data Cleansing<br/>
   3. Calculating RFM Metrics<br/>
   4. Data Preprocessing<br/>
   5. K-Means Clustering<br/>
   6. Conclusion
</font><br/>

**<font size=4 color='red'>🔗 Check out Full Code </font>**<b><a href="https://github.com/lungyongmi/Segment_Customers_with_RFM_and_KMeans/blob/main/Segment%20Customers%20with%20RFM%20and%20K-Means_Full%20Code.ipynb">here.</a></b>

### <font color='blue'> 1. Reading and Exploring Data </font>
**<font size=3> a. Import Libraries </font>**
```python
# Import Libraries for Dataframe and Visualization
import numpy as np
import pandas as pd
import datetime as dt

import matplotlib.pyplot as plt
import seaborn as sns

# Import Libraries for Machine Learning Algorithm
import sklearn
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans

import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)
```

**<font size=3> b. Read and Explore Data </font>**<br/> <font size=3> There are 8 columns and 541909 rows. </font>
```python
df = pd.read_excel('Online Retail.xlsx')
df.head()
```
<img src='/images/P1_01.png' width='90%' height='90%'>
<br/>

### <font color='blue'> 2. Data Cleansing </font>
**<font size=3> a. Check and Drop Missing Values and Duplicates </font>**<br/>
**<font size=3> b. Check and Change Data Types </font>**<br/> <font size=3> The data type of ‘CustomerID’ should be object type. </font>  
```python
df.dtypes
df['CustomerID'] = df['CustomerID'].astype('int').astype('str')
```

**<font size=3> c. Exclude Noisy Data </font>**<br/> <font size=3> Remove negative and 0 values in ‘Quantity’ and ‘UnitPrice’. </font>
```python
df.describe()
df = df[(df['Quantity'] > 0) & (df['UnitPrice'] > 0)]
```

### <font color='blue'> 3. Calculating RFM Metrics  🔗 <a href="https://github.com/lungyongmi/Segment_Customers_with_RFM_and_KMeans/blob/main/Segment%20Customers%20with%20RFM%20and%20K-Means_Full%20Code.ipynb">Full Code</a> </font>
**<font size=3> a. RFM represents Recency, Frequency and Monetary.<br/> &nbsp;&nbsp;&nbsp;&nbsp; RFM is a model used to segment customers base by their purchasing patterns.</font>**<br/>
<font size=3> &nbsp;&nbsp;&nbsp;&nbsp; R (Recency) : How long ago since the last purchase of each customer.<br/> &nbsp;&nbsp;&nbsp;&nbsp; F (Frequency) : How often each customer make purchases.<br/> &nbsp;&nbsp;&nbsp;&nbsp; M (Monetary) : Total amount of money each customer spends.</font><br/>


**<font size=3> b. RFM Score </font>**<br/> <font size=3> Rank each customer in these three categories on a scale of 1 to 4 (higher number, better result). </font>
```python
r_labels = range(4, 0, -1)
f_labels = range(1, 5)
m_labels = range(1, 5)

r_quartiles = pd.qcut(rfm['Recency'], q = 4, labels = r_labels)
f_quartiles = pd.qcut(rfm['Frequency'], q = 4, labels = f_labels)
m_quartiles = pd.qcut(rfm['Monetary'], q = 4, labels = m_labels)

rfm = rfm.assign(R = r_quartiles,
                 F = f_quartiles,
                 M = m_quartiles)

def rfm_seg(x):
    return str(x['R']) + str(x['F']) + str(x['M'])

rfm['RFM'] = rfm.apply(rfm_seg, axis = 1)
rfm['RFM_Score'] = rfm[['R', 'F', 'M']].sum(axis = 1)
rfm.head()
```
<img src='/images/P1_02.png' width='75%' height='75%'>
<br/>


### <font color='blue'> 4. Data Preprocessing  🔗 <a href="https://github.com/lungyongmi/Segment_Customers_with_RFM_and_KMeans/blob/main/Segment%20Customers%20with%20RFM%20and%20K-Means_Full%20Code.ipynb">Full Code</a> </font>
**<font size=3> a. Detect and Remove Outliers Using the IQR Method </font>**

```python
x = ['Recency', 'Frequency', 'Monetary']
f, ax = plt.subplots(figsize = (8, 6))

sns.boxplot(data = rfm[x], palette = 'Set2')
plt.yscale('log')
plt.title('Outliers Variable Distribution', fontsize = 12)
plt.xlabel('RFM')
plt.ylabel('Range')
plt.show()
```
<br/>
<img src='/images/P1_03.png' width='75%' height='75%'>
<br/>

**<font size=3> b. RFM Segmentation by RFM Score </font>**<br/> <font size=3> Segment customers into 6 groups by RFM Score. </font>

<style>
table th:first-of-type {
    width: 2cm;
}
table th:nth-of-type(2) {
    width: 3cm;
}
</style>

|Segment        |RFM Score| 
|--------------:| |:--------|
| champions          |> 10|
| potential_loyalists|9-10| 
| need_attention     |8   | 
| about_to_sleep     |7   | 
| at_risk            |5-6 | 
| hibernating        |< 4 | 
 
<br/> 
<img src='/images/P1_04.png' width='75%' height='75%'>
<br/>

**<font size=3> c. Standardization </font>**<br/> <font size=3> It’s important to rescale RFM values so that they have a comparable scale. </font>

```python
rfm_RFM = rfm[['Recency', 'Frequency', 'Monetary']]

scaler = StandardScaler()
rfm_standard = scaler.fit_transform(rfm_RFM)
rfm_standard = pd.DataFrame(rfm_standard)
rfm_standard.columns = ['Recency', 'Frequency', 'Monetary']
rfm_standard.head()
```

**<font size=3> d. Find the Optimal Number of Clusters Using Elbow Method </font>**
```python
wcss =[]
range_k = range(1, 11)

for k in range_k:
    kmeans = KMeans(n_clusters = k, random_state = 46)
    kmeans.fit(rfm_standard)
    wcss.append(kmeans.inertia_)

f, ax = plt.subplots(figsize = (8, 6))

plt.plot(range_k, wcss, marker = 'o', color = 'black')
plt.title('The Optimal Number of Clusters', fontsize = 12)
plt.xlabel('Numbers of Clusters, K', fontsize = 10)
plt.ylabel('Inertia', fontsize = 10)
plt.xticks(range_k)
plt.show()
```
<br/>
<img src='/images/P1_05.png' width='75%' height='75%'>
<br/>

### <font color='blue'> 5. K-Means Clustering  🔗 <a href="https://github.com/lungyongmi/Segment_Customers_with_RFM_and_KMeans/blob/main/Segment%20Customers%20with%20RFM%20and%20K-Means_Full%20Code.ipynb">Full Code</a> </font>

```python
# Choose K=3
kmeans = KMeans(n_clusters = 3, random_state = 46)
kmeans.fit(rfm_standard)

cluster_labels = kmeans.labels_
rfm_k3 = rfm.assign(Cluster = cluster_labels)

rfm_k3.groupby(['Cluster', 'Segment']).agg({'CustomerID':'count',
'RFM_Score':'mean'}).round(2)
```
<img src='/images/P1_06.png' width='45%' height='45%'>
<br/>

```python
rfm_melt = pd.melt(rfm_standard,
                   id_vars = ['CustomerID', 'Cluster'],
                   value_vars = ['Recency', 'Frequency', 'Monetary'],
                   var_name = 'Metric',
                   value_name = 'Value')

f, ax = plt.subplots(figsize = (8, 6))

sns.lineplot(data = rfm_melt, x = 'Metric', y = 'Value',
             hue = 'Cluster', 
             palette = sns.color_palette('Set2', n_colors=3),
             sort = False)

plt.title('RFM of Clusters', fontsize = 12)
plt.xlabel('Metric')
plt.ylabel('Value')
plt.show()
```
<img src='/images/P1_07.png' width='75%' height='75%'>
<br/>

```python
from mpl_toolkits.mplot3d import Axes3D

colors = ['#66C3A5', '#FC8D62', '#8DA0CB']
fig = plt.figure(figsize = (8, 6))
ax = plt.axes(projection = '3d')

for i in range(kmeans.n_clusters):
    rfm_standard = rfm_k3[rfm_k3['Cluster'] == i]
    ax.scatter(rfm_standard['Recency'],
               rfm_standard['Frequency'],
               rfm_standard['Monetary'],
               s = 10,
               label = 'Cluster' + str(i),
               c = colors[i])

ax.set_xlabel('Recency')
ax.set_ylabel('Frequency')
ax.set_zlabel('Monetary')

plt.tight_layout()
plt.legend()
plt.show()
```
<img src='/images/P1_08.png' width='75%' height='75%'>
<br/>

### <font color='blue'> 6. Conclusion 🔗 <a href="https://github.com/lungyongmi/Segment_Customers_with_RFM_and_KMeans/blob/main/Segment%20Customers%20with%20RFM%20and%20K-Means_Full%20Code.ipynb">Full Code</a> </font>

* <font size=3>CLUSTER 0 :<br/> 約佔整體的 64%，RFM 平均介於 2-3，多為 potential_loyalists 和 need_attention 之客戶，建議列為目標客戶。</font>
* <font size=3>CLUSTER 1 :<br/> 約佔整體的 11%，RFM 平均接近 4，多為 champions 客戶，須維持與其之間的良好關係，以及其對於品牌的忠誠度。</font>
* <font size=3>CLUSTER 2 :<br/> 約佔整體的 25%，RFM 平均小於 2，多為 hibernating 客戶。</font> 
<br/>
<iframe title="Report Section" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiNDU2NTQ3ZjQtZGZkMi00ZDVlLWJiYTUtYzY3MTYyYTdmMDgwIiwidCI6IjE0ZmM0NDhkLWYxOWEtNDQ4ZS04MjRhLWQ4MmM3MWFhOTg4ZSIsImMiOjEwfQ%3D%3D" frameborder="0" allowFullScreen="true"></iframe>