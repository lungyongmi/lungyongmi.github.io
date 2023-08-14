---
title: "Segment Customers with RFM and K-Means"
#excerpt: "Short description of portfolio item number 1<br/><img src='/images/500x300.png'>"
#collection: portfolio
---
 

### Project Goal：<br/>
<font size=3> Segment customers with RFM and K-Means in order to target customers efficiently<br/>
透過 RFM 模型與  KMeans 演算法進行客戶分群，找出目標客戶。<br/> </font>

### Dataset Overview：<br/>
<font size=3> This is a transnational data set which contains all the transactions occurring between 2010/12/01 and 2011/12/09 for a UK-based and registered non-store online retail.The company mainly sells unique all-occasion gifts. Many customers of the company are wholesalers. </font>

### Data Source：
<font size=3> [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail) </font>

### Tools：
<font size=3> Python, Power BI </font>

### Analysis Process：
<font size=3> 
   1. Reading and Exploring Data<br/>
   2. Data Cleaning<br/>
   3. Calculating RFM Metrics<br/>
   4. Data Preprocessing<br/>
   5. K-Means Clustering<br/>
   6. Data Visualization<br/>
</font>

<font size=3, color='red'> _🔗 Check out Full Code [here]()._ </font>


1. Reading and Exploring Data (🔗 [Full Code]())
    a. Import Libraries
    `# Import Libraries for Dataframe and Visualization`<br/>
    `import numpy as np`<br/>
    `import pandas as pd`<br/>
    `import datetime as dt`<br/>

    `import seaborn as sns`<br/>
    `import matplotlib.pyplot as plt`<br/>

    `# Import Libraries for Machine Learning Algorithm`<br/>
    `import sklearn`<br/>
    `from sklearn.preprocessing import StandardScaler`<br/>
    `from sklearn.cluster import KMeans`<br/>

    `import warnings`<br/>
    `warnings.simplefilter(action='ignore', category=FutureWarning)`<br/>