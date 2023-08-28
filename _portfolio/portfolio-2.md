---
title: "Personal Finance Dashboard"
#excerpt: "Short description of portfolio item number 1<br/><img src='/images/500x300.png'>"
collection: portfolio
---

### Project Goal: <br/>
<font size=3> 設計記帳表單以控管個人年度預算與支出，並透過資料視覺化分析每月支出狀況。 </font>

### Dataset Overview: <br/>
<font size=3> 此資料集為本人 2023 年度支出之紀錄。 </font>

### Tools: Excel

### Analysis Process:
<font size=3> 
   1. 記帳表單設計<br/>
   2. 資料清理<br/>
   3. 資料視覺化與分析<br/>
</font><br/>

### <font color='blue'> 1. 記帳表單設計 </font>
**<font size=3> 共設計 4 種工作表，分為每月紀錄、支出類型、年度預算與個人資產。 </font>**

**<font size=3> a. 每月紀錄</font>**
<br/>

<img src='/images/P2_01.png' width='130%' height='130%'>
<font size=3> 以月為單位，紀錄每筆財務明細，結構與設計如下：<br/>
<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➊ 月總整理：包含當月總收入、支出與存款<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➋ 年度預算：使用 SPARKLINE 函數，製作各類別已支出金額占預算之進度條<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➌ 無支出日：使用 SQUENCE 函數和條件式格式設定，製作行事曆<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➍ 支出方式：紀錄現金與信用卡的支出金額<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➎ 本月活動：事先預估本月預計參與活動之所需預算<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➏ 收入明細：紀錄所有收入項目<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➐ 每週明細：使用 SUMIFS 函數紀錄每週支出，以及各類別已支出金額占預算之比例<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➑ 固定支出明細：紀錄月繳之固定支出明細<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ➒ 支出明細：紀錄所有支出明細，包含日期、內容、金額、子類別、類別與方式 </font>
<br/>
**<font size=3> b. 支出類型 </font>** <br/><font size=3> 將支出類型分為 6 個類別，各類別另有子類別，以利支出分析。</font>

**<font size=3> c. 年度預算 </font>** <br/><font size=3> 於年初訂定今年度預算，此工作表用於統整每月支出，追蹤各類別預算與年度預算。</font>
<br/>
<img src='/images/P2_02.png'>

**<font size=3> d. 資產總表 </font>** <br/><font size=3> 定期更新各帳戶金額，用以控管個人財務狀況。<br/></font>

### <font color='blue'> 2. 資料清理 </font>
<font size=3>
&nbsp;&nbsp;&nbsp;&nbsp; ・彙整各月支出明細<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ・調整並確認各欄資料之格式<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ・使用 XLOOKUP 函數自動顯示各列子類別所屬類別<br/> </font>

### <font color='blue'> 3. 資料視覺化與分析 </font>
<img src='/images/P2.gif' width='130%' height='130%'>
<br/>
<font size=3>
藉由記帳表單與 Personal Finance Dashboard，成功控管總支出金額，符合年度預算。<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ・記帳表單設計之「年度預算進度條」有助於警惕各類別的支出狀況<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ・「娛樂」和「購物」類別的支出金額已接近預算上限<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ・總支出金額較去年同期減少近 10,000 元<br/>


