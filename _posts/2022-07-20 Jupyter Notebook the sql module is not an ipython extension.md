---
title: Jupyter Notebook the sql module is not an ipython extension
categories: Programming Language
tags:
  - jupyter
  - ipython
  - python
  - sql
date: 2022-07-20 01:29:59
---

在 Jupyter Notebook 中执行

```python
%load_ext sql
```

显示 the sql module is not an ipython extension。参考[python - SQL iPython Magic Extension won't load - Stack Overflow](https://stackoverflow.com/questions/34967288/sql-ipython-magic-extension-wont-load) 和 [python - ipython can't load sql - Stack Overflow](https://stackoverflow.com/questions/36139459/ipython-cant-load-sql)，执行

```bash
pip install ipython-sql
```

成功安装 ipython-sql 后执行，仍然显示该错误。将 %load_ext sql 改为

```python
%reload_ext sql
```

可以成功加载 SQL 扩展，但是只要重启 Jupyter Notebook 使用的 Python 内核，重新执行 %load_ext sql 即可成功加载 SQL 扩展。
