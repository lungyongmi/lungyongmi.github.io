---
title: "Project 1：Customer Segmentation with RFM Analysis Using K-Means"
collection: portfolio
---

### Project Goal: <br/>
<font size=3> Segment customers in order to target customers efficiently. <br/>
協助公司了解客戶樣貌，找出目標客群，進行分眾行銷分析。<br/> </font>

### Dataset Overview: <br/>
<font size=3> This is a transnational data set which contains all the transactions occurring between 2010/12/01 and 2011/12/09 for a UK-based and registered non-store online retail. The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers. </font>

### Data Source: <a href="https://archive.ics.uci.edu/dataset/352/online+retail">UCI Machine Learning Repository</a>
### Tools: Python, Power BI

### Analysis Process:
<font size=3> 
   1. Load Data<br/>
   2. Data Cleansing<br/>
   3. Explore Data & Cohort Analysis<br/>
   4. RFM Analysis<br/>
   5. K-Means Clustering<br/>
   6. Conclusion
</font><br/>

**<font size=4 color='red'>🔗 Check out Full Code </font>**<b><a href="https://github.com/lungyongmi/Segment_Customers_with_RFM_and_KMeans/blob/main/Segment%20Customers%20with%20RFM%20and%20K-Means_Full%20Code.ipynb">here.</a></b>

### <font color='blue'> 1. Load Data </font>
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

**<font size=3> b. Load Data </font>**<br/> <font size=3> There are 8 columns and 541909 rows. </font>
```python
df = pd.read_excel('Online Retail.xlsx')
df.head()
```
<img src='/images/P1_01.png' width='90%' height='90%'>
<br/>

### <font color='blue'> 2. Data Cleansing </font>
**<font size=3> a. Check and Drop Missing Values </font>**<br/>
**<font size=3> b. Check and Drop Duplicates </font>**<br/>
**<font size=3> c. Check and Change Data Types </font>**<br/> <font size=3> The data type of ‘CustomerID’ should be object type. </font>  
```python
df.dtypes
df['CustomerID'] = df['CustomerID'].astype('int').astype('str')
```

**<font size=3> d. Exclude Noisy Data </font>**<br/> <font size=3> Remove negative and 0 values in ‘Quantity’ and ‘UnitPrice’ columns. </font>
```python
df.describe()
df = df[(df['Quantity'] > 0) & (df['UnitPrice'] > 0)]
```

### <font color='blue'> 3. Explore Data & Cohort Analysis </font>
**<font size=3> Top 10 Countries </font>**
```python
df['Country'].value_counts(normalize = True).head(10).mul(100).round(1)
```
<img src='/images/P1_02.png' width='30%' height='30%'>
<br/>

**<font size=3> Monthly Transaction and Transaction Amount </font>**
<img src='/images/P1_03.png' width='90%' height='90%'>
<br/>

**<font size=3> Corhort Analysis </font>** <br/>
<font size=3> Create a few labels for cohort analysis: <br/> &nbsp;&nbsp;&nbsp;&nbsp; Invoice Period: The year and month of each transaction.<br/> &nbsp;&nbsp;&nbsp;&nbsp; Cohort Group: The year and month of a particular customer’s first purchase.<br/> &nbsp;&nbsp;&nbsp;&nbsp; Cohort Index: A number represents the number of months passed since the first purchase. <font/><br/>
<img src='/images/P1_04.png' width='90%' height='90%'>
<br/>

### <font color='blue'> 4. RFM Analysis </font>
**<font size=3> a. Calculate RFM Metrics<br/> &nbsp;&nbsp;&nbsp;&nbsp;RFM represents Recency, Frequency and Monetary.<br/> &nbsp;&nbsp;&nbsp;&nbsp;RFM is a model used to segment customers base by their purchasing patterns.</font>** <br/>
<font size=3> &nbsp;&nbsp;&nbsp;&nbsp; R (Recency) : How long ago since the last purchase of each customer.<br/> &nbsp;&nbsp;&nbsp;&nbsp; F (Frequency) : How often each customer make purchases.<br/> &nbsp;&nbsp;&nbsp;&nbsp; M (Monetary) : Total amount of money each customer spends.</font><br/>

**<font size=3> b. Detect and Remove Extreme Outliers using the IQR Method </font>**<br/>
**<font size=3> c. RFM Score </font>**<br/> <font size=3> Rank each customer in these three categories on a scale of 1 to 4 (higher number, better result). </font> <br/>

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
rfm.reset_index(inplace = True)
rfm.tail()
```
<img src='/images/P1_05.png' width='90%' height='90%'>
<br/>


### <font color='blue'> 5. K-Means Clustering </font><br/>
**<font size=3> a. Data Preprocessing: Data Scaling & Standardization </font>**<br/>

```python
# Undkew data with log scale
rfm_log = rfm[['Recency', 'Frequency', 'Monetary']].apply(np.log, axis = 1).round(3)

# Standardization
scaler = StandardScaler()
rfm_standard = scaler.fit_transform(rfm_log)
rfm_standard = pd.DataFrame(rfm_standard)
rfm_standard.columns = ['Recency', 'Frequency', 'Monetary']
rfm_standard.head()
```
**<font size=3> b. Find the Optimal Number of Clusters Using Elbow Method and Silhouette Coefficient </font>**
```python
# Elbow Method
wcss =[]
range_k = range(1, 11)

for k in range_k:
    kmeans = KMeans(n_clusters = k, random_state = 46)
    kmeans.fit(rfm_standard)
    wcss.append(kmeans.inertia_)

# Silhouette Coefficient
from sklearn.metrics import silhouette_score
range_k = range(2, 11)
silhouette_avg = []

for k in range_k:
    kmeans = KMeans(n_clusters = k, init='k-means++', random_state = 1)
    kmeans.fit(rfm_standard)
    cluster_labels = kmeans.labels_
    silhouette_avg.append(silhouette_score(rfm_standard, cluster_labels))
```

**<font size=3> c. K-Means Clustering </font>**
```python
# Choose K=4
kmeans = KMeans(n_clusters=4, random_state=46)
kmeans.fit(rfm_standard)

cluster_labels = kmeans.labels_
rfm_k4= rfm.assign(K_Cluster = cluster_labels)

rfm_k4.groupby('K_Cluster').agg({'Recency':'mean',
                                 'Frequency':'mean',
                                 'Monetary':'mean',
                                 'RFM_Score':'mean'}).round(1)
```
<img src='/images/P1_06.png' width='60%' height='60%'>
<br/>

```python
rfm_melt = pd.melt(rfm_standard,
                   id_vars = ['CustomerID', 'Cluster'],
                   value_vars = ['Recency', 'Frequency', 'Monetary'],
                   var_name = 'Metric',
                   value_name = 'Value')

f, ax = plt.subplots(figsize = (8,6))
sns.lineplot(x = 'Metric', y = 'Value', hue = 'K_Cluster', data = rfm_melt, palette = "Set2")
plt.style.use('ggplot')
plt.title('Snake Plot of RFM')
plt.xlabel('Value')
plt.ylabel('Metric')
plt.show()
```
<img src='/images/P1_07.png' width='75%' height='75%'>
<br/>

### <font color='blue'> 6. Conclusion </font><br/>
<font size = 3>
1. 數據顯示，2011年上半年客戶留存率平均比下半年高，建議了解上下半年度行銷策略上是否有變動。<br/>
2. 根據 11 月的交易數量與金額，以及交易數量前十大產品顯示，聖誕節慶前的 11 月為創造最大收益的時機。<br/>
3. 根據 KMeans 之分群結果，客戶類型與行銷策略建議如下：</font><br/>
<img src='/images/P1_08.png' width='90%' height='90%'>
<br/>

<iframe title="Report Section" width="800" height="486" src="https://app.powerbi.com/view?r=eyJrIjoiNDU2NTQ3ZjQtZGZkMi00ZDVlLWJiYTUtYzY3MTYyYTdmMDgwIiwidCI6IjE0ZmM0NDhkLWYxOWEtNDQ4ZS04MjRhLWQ4MmM3MWFhOTg4ZSIsImMiOjEwfQ%3D%3D" frameborder="0" allowFullScreen="true"></iframe>