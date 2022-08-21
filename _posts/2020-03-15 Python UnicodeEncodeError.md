---
title: Python解决gbk、utf-8、gb2312的UnicodeEncodeError与UnicodeDecodeError乱码解码的两种方法
categories: CSDN补档
tags:
  - python
  - matlab
  - codec
  - regex
  - gbk
abbrlink: 40036
date: 2020-03-15 11:09:16
---

Python在处理文字数据时往往会面对文字中乱码的困扰，这其中往往还包含了神奇的双字节字符和多字节字符，我们只想删除这些乱码而不想去掉字符串中的任何普通符号，可以使用正则表达式处理（以MATLAB为例）：

```
matchStr=unique(regexp(str{i},'[^\x00-\xff]','match'))
```

然后使用strrep函数即可删除这些非双字节（非ASCII）字符，特别地，[\u4e00-\u9fa5]可以匹配中文字符。但是Python读取替换过后的字符串还是报错：

```
UnicodeDecodeError: 'gbk' codec can't decode byte xxxx in position
```

如果你设置utf-8解码或gb2312解码仍然会报错。问题在于正则表达式可以处理ASCII与Unicode编码，但**不能处理GBK编码**，遇上GBK编码的字符串是无解的（参考[正则表达式（二）：Unicode诸问题（上）](https://www.cnblogs.com/simpman/archive/2013/02/22/2921907.html)），而我们收到一组数据时往往不能对数据提供方要求说你们得给Unicode编码的字符，所以我们要忽略掉这些不能被正确编码解码的字符。

方式一：读取文件对象时忽略错误

```
with open(file_path, errors='ignore') as file_object:
    lines = file_object.readlines()
```

方式二：读取表格等数据类型时使用ISO-8859-1编码（其中header默认值即为'infer'，sep相当于MATLAB中的'Delimiter'分隔符）

```
import pandas as pd
 
table1 = pd.read_table(file_path, sep='\t', header='infer', encoding='ISO-8859-1')
```

成功解决。