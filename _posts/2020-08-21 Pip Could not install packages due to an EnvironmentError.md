---
title: pip升级会导致Could not install packages due to an EnvironmentError：[WinError
  5]拒绝访问错误的原因
categories: CSDN补档
tags:
  - python
  - pip
abbrlink: 17887
date: 2020-08-21 09:33:54
---

Python安装时可勾选安装pip，安装完成后执行

```
pip -v
```

或

```
pip --version
```

可成功显示版本号（以Python 3.8.5为例）

```
pip 20.1.1 from d:\program_files\python38\lib\site-packages\pip (python 3.8)
```

然而，如果执行

```
pip install --upgrade pip
```

会报错

```
Collecting pip
  Using cached pip-20.2.2-py2.py3-none-any.whl (1.5 MB)
Installing collected packages: pip
  Attempting uninstall: pip
    Found existing installation: pip 20.1.1
    Uninstalling pip-20.1.1:
ERROR: Could not install packages due to an EnvironmentError: [WinError 5] 拒绝访问。: 'd:\\program_files\\python38\\scripts\\pip.exe'
Consider using the `--user` option or check the permissions.
```

该操作会导致原有的pip模块被破坏，再执行获取版本号命令会显示

```
Traceback (most recent call last):
  File "d:\program_files\python38\lib\runpy.py", line 194, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "d:\program_files\python38\lib\runpy.py", line 87, in _run_code
    exec(code, run_globals)
  File "D:\Program_Files\Python38\Scripts\pip.exe\__main__.py", line 4, in <module>
ModuleNotFoundError: No module named 'pip'
```

如执行

```
python -m pip install --upgrade pip
```

会显示

```
D:\Program_Files\Python38\python.exe: No module named pip
```

只能重装pip或Python，所以切勿执行非--user选项的升级命令。