---
title: "Project 2：Analyze Apps Using SQL"
collection: portfolio
---

### Project Goal: <br/>
<font size=3> Find insights for the app developer who needs to decide what type of app to develop.<br/> </font>

### Dataset Overview: <br/>
<font size=3> Contain Information of apps on the Apple Store, including names, price, ratings, number of supporting languages, etc. </font>

### Data Source: <a href="https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps">Kaggle</a>
### Tools: MySQL


### Analysis Process:
<font size=3> 
   1. Import Data <br/>
   2. Read and Explore Data <br/>
   3. Data Analysis <br/>
   4. Find Insights <br/>
</font><br/>

**<font size=4 color='red'>🔗 Check out Full Code </font>**<b><a href="https://github.com/lungyongmi/Analyze_Apps_Using_SQL">here.</a></b>

**<font size=3> About Dataset </font>**
<font size=3>
01 id : app ID <br/>
02 track_name: app name <br/>
03 size_bytes: size in bytes <br/>
05 price： Price amount <br/>
06 rating_count_tot: user rating counts (for all version) <br/>
08 user_rating: average user rating value (for all version)
12 prime_genre: primary Genre <br/>
13 sup_devices.num: number of supporting devices <br/>
14 ipadSc_urls.num: number of screenshots showed for display <br/>
15 lang.num: number of supported languages <br/></font>

### <font color='blue'> 1. Import Data 🔗 <a href="https://github.com/lungyongmi/Analyze_Apps_Using_SQL/blob/main/Import_Data.sql">Full Code</a> </font>
**<font size=3> a. Create Tables and Import Data 🔗 </font>**

```sql
-- Create Tables and Import Data
DROP TABLE IF EXISTS applestore;
CREATE TABLE applestore(
    id INT NOT NULL PRIMARY KEY,
    track_name VARCHAR(255),
    size_bytes BIGINT,
    currency VARCHAR(255),
    price DECIMAL,
    rating_count_tot INT,
    rating_count_ver INT,
    user_rating DECIMAL,
    user_rating_ver DECIMAL,
    ver VARCHAR(255),
    cont_rating VARCHAR(255),
    prime_genre VARCHAR(255),
    sup_devices_num INT,
    ipadSc_urls_num INT,
    lang_num INT,
    vpp_lic INT
);

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/applestore.csv'
INTO TABLE applestore
CHARACTER SET latin7
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
```

### <font color='blue'> 2. Read and Explore Data 🔗 <a href="https://github.com/lungyongmi/Analyze_Apps_Using_SQL/blob/main/Data_Analysis.sql">Full Code</a> </font>
**<font size=3> a. Explore Data </font>**
<font size=3> 7197 rows and 16 columns </font>

```sql
-- Explore Data
SELECT *
FROM applestore;
```
<img src='/images/P2_01.png' width='100%' height='100%'>
<br/>

**<font size=3> b. Number of Apps by Genre </font>**
```sql
-- Number of Apps by Genre
SELECT prime_genre AS Genre,
       COUNT(*) AS Num
FROM applestore
GROUP BY Genre
ORDER BY Num DESC;
```
<img src='/images/P2_02.png' width='100%' height='100%'>
<br/>

**<font size=3> c. Average of App Ratings and Paid App Price </font>**
```sql
-- Average of App Ratings and Paid App Price
SELECT ROUND(AVG(user_rating), 1) AS AvgRating,
       ROUND((SELECT AVG(price) FROM applestore WHERE price > 0), 1) AS AvgPrice
FROM applestore;
```
<img src='/images/P2_03.png' width='100%' height='100%'>
<br/>

### <font color='blue'> 3. Data Analysis 🔗 <a href="https://github.com/lungyongmi/Analyze_Apps_Using_SQL/blob/main/Data_Analysis.sql">Full Code</a> </font>
**<font size=3> a. Check whether paid apps have higher ratings than free apps </font>**
```sql
-- Check whether paid apps have higher ratings than free apps
SELECT 
       CASE
	   WHEN price > 0 THEN 'Paid'
	   ELSE 'Free'
	END AS AppType,
        ROUND(AVG(user_rating), 1) AS Rating
FROM applestore
GROUP BY AppType;
```
<img src='/images/P2_04.png' width='100%' height='100%'>
<br/>

**<font size=3> b. Check if apps with more supporting languages have higher rating </font>**
```sql
-- Check if apps with more supporting languages have higher rating
SELECT 
       CASE
	   WHEN lang_num < 10 THEN '<10 languages'
	   WHEN lang_num BETWEEN 10 AND 30 THEN '10-30 languages'
	   ELSE '>30 languages'
	END AS lang_type,
        ROUND(AVG(user_rating), 1) AS AvgRating
FROM applestore
GROUP BY lang_type
ORDER BY AvgRating DESC;
```
<img src='/images/P2_05.png' width='100%' height='100%'>
<br/>

**<font size=3> c. Check correlation between app screenshot and rating </font>**
```sql
-- Check if apps with more supporting languages have higher rating
SELECT 
       CASE
	   WHEN ipadSc_urls_num < 1 THEN 'No Screenshot'
	   WHEN ipadSc_urls_num BETWEEN 1 AND 3 THEN '1-3 Screenshot'
	   ELSE '4-5 Screenshot'
	END AS ScrnType,
        ROUND(AVG(user_rating), 1) AS AvgRating
FROM AvgRating
GROUP BY ScrnType;
```
<img src='/images/P2_06.png' width='100%' height='100%'>
<br/>

**<font size=3> d. Top Rating App in Each Genre </font>**
```sql
-- Top Rating App in Each Genre
WITH top_app AS(
SELECT 
       prime_genre,
       track_name,
       user_rating,
       rating_count_tot,
       RANK() OVER(PARTITION BY prime_genre ORDER BY user_rating DESC, rating_count_tot DESC) AS tot_r
FROM applestore)

SELECT 
       prime_genre AS Genre,
       track_name AS App,
       user_rating AS Rating
FROM top_app
WHERE tot_r = 1
ORDER BY rating_count_tot DESC;
```
<img src='/images/P2_07.png' width='100%' height='100%'>
<br/>

### <font color='blue'> 4. Finding Insights </font>

<font size=3>
・The new app should set goal for an average rating above 3.8. <br/>
・Paid Apps have better ratings, and the average price is 4.0. <br/>
・Games and Entertainment genre have high competition. <br/>
・The average ratings in Catelog, Medical and Navigation genre are very low.<br/>
・Apps with 4-5 screenshots showed for display and 10-30 supporting languages have better ratings. </font>