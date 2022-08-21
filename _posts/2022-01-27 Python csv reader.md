---
title: Python csv reader 跳过第一行表头
categories: Programming Language
tags:
  - python
  - csv
date: 2022-01-27 16:44:05
---

*在中文互联网上搜出来都是什么奇奇怪怪的*，Google 第一条就正解（

太长不看：**用** *next(csvreader)*

参考：
Skip the header of a file with Python's CSV reader (evanhahn.com)
https://evanhahn.com/python-skip-header-csv-reader/

如果你的 CSV 长这样：

```
Date,Description,Amount
2015-01-03,Cakes,22.55
2014-12-28,Rent,1000
2014-12-27,Candy Shop,12
... 
```

跳过表头的方法：

```python
with open("mycsv.csv", "r") as csvfile:
    csvreader = csv.reader(csvfile)
    next(csvreader)

    for row in csvreader:
        # 处理行数据
```
你还可以用 [DictReader](https://docs.python.org/3/library/csv.html#csv.DictReader)：

```python
with open("mycsv.csv", "r") as csvfile:
    csvreader = csv.DictReader(csvfile)
    for row in csvreader:
        print(row["Date"], row["Description"], row["Amount"])
```
