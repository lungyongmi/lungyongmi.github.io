---
title: "Project 3：Personal Finance Dashboard"
collection: portfolio
---

### Project Goal: <br/>
<font size=3> Design financial tracker to manage annual budget and expense, and use data visualization to find insights.<br/>設計記帳表單以控管個人年度預算與支出，並透過資料視覺化分析每月支出狀況。<br/> </font>

Design accounting forms to control personal annual budgets and expenditures, and use data to visually analyze monthly expenditures.

### Dataset Overview: <br/>
<font size=3> 此資料集為本人 2023 年度支出之紀錄。 </font>

### Tools: Excel

### Analysis Process:
<font size=3> 
   1. 記帳表單設計<br/>
   2. 資料清理<br/>
   3. 資料視覺化與分析<br/>
   4. 總結
</font><br/>

### <font color='blue'> 1. 記帳表單設計 </font>
<font size=3> 共設計 4 種工作表，分為 a. 每月紀錄、b. 支出類型、c. 年度預算與 d. 個人資產。 </font>

**<font size=3> a. 每月紀錄</font>**
<img src='/images/P3_01.png' width='130%' height='130%'>
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
<img src='/images/P3_02.png'>

**<font size=3> d. 個人資產 </font>** <br/><font size=3> 定期更新各帳戶金額，用以控管個人財務狀況。<br/></font>

### <font color='blue'> 2. 資料清理 </font>
<font size=3>
&nbsp;&nbsp;&nbsp;&nbsp; ・彙整各月支出明細<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ・調整並確認各欄資料之格式<br/>
&nbsp;&nbsp;&nbsp;&nbsp; ・使用 XLOOKUP 函數自動顯示各列子類別所屬類別<br/> 
&nbsp;&nbsp;&nbsp;&nbsp; ・建立樞紐分析表與圖表<br/> 
</font>

### <font color='blue'> 3. 資料視覺化 </font>
<font size=3> 使用 Excel 製作 Personal Finance Dashboard，分為 Overview 與 Monthly：</font>

**<font size=3> Overview: </font>**<br/><font size=3>・Total Expenses<br/>
・Expenses by Month<br/>
・Expenses by Category<br/>
・TOP 5 Expenses</font>
<br/>

**<font size=3> Monthly: </font>**<br/><font size=3> ・Monthly Expenses by Category <br/>
・Top 3 Sub-category by Category<br/>
・Monthly Overview<br/></font>
<br/>
<img src='/images/P3.gif' width='130%' height='130%'>
<br/>

### <font color='blue'> 4. 總結 </font>
<font size=3>
・記帳表單之設計，得以定期檢視財務狀況，成功控管年度預算，總支出金額較去年同期減少近 10,000 元。 <br/>
・Personal Finance Dashboard 則有助於階段性檢核整體與各類別支出狀況。 </font>