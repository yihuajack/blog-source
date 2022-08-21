---
title: Python安装wordcloud报错error：Microsoft Visual C++ 14.0 is required的解决方法
categories: CSDN补档
tags:
  - python
  - visual studio
  - pip
  - visual c++
  - .net
abbrlink: 37588
date: 2020-03-09 11:27:11
---

 使用PyCharm的虚拟环境安装wordcloud时出现错误：

```
Collecting wordcloud
  Using cached wordcloud-1.6.0.tar.gz (214 kB)
Requirement already satisfied: numpy>=1.6.1 in d:\documents\programming\python\venv\lib\site-packages (from wordcloud) (1.18.1)
Requirement already satisfied: pillow in d:\documents\programming\python\venv\lib\site-packages (from wordcloud) (7.0.0)
Requirement already satisfied: matplotlib in d:\documents\programming\python\venv\lib\site-packages (from wordcloud) (3.2.0)
Requirement already satisfied: kiwisolver>=1.0.1 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (1.1.0)
Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (2.4.6)
Requirement already satisfied: cycler>=0.10 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (0.10.0)
Requirement already satisfied: python-dateutil>=2.1 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (2.8.1)
Requirement already satisfied: setuptools in d:\documents\programming\python\venv\lib\site-packages (from kiwisolver>=1.0.1->matplotlib->wordcloud) (46.0.0)
Requirement already satisfied: six in c:\users\<username>\appdata\roaming\python\python38\site-packages (from cycler>=0.10->matplotlib->wordcloud) (1.14.0)
Installing collected packages: wordcloud
    Running setup.py install for wordcloud: started
    Running setup.py install for wordcloud: finished with status 'error'
 
    ERROR: Command errored out with exit status 1:
     command: 'D:\Documents\Programming\Python\venv\Scripts\python.exe' -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'C:\\Users\\<username>\\AppData\\Local\\Temp\\pycharm-packaging\\wordcloud\\setup.py'"'"'; __file__='"'"'C:\\Users\\<username>\\AppData\\Local\\Temp\\pycharm-packaging\\wordcloud\\setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record 'C:\Users\<username>\AppData\Local\Temp\pip-record-np4ix5r5\install-record.txt' --single-version-externally-managed --compile --install-headers 'D:\Documents\Programming\Python\venv\include\site\python3.8\wordcloud'
         cwd: C:\Users\<username>\AppData\Local\Temp\pycharm-packaging\wordcloud\
    Complete output (22 lines):
    running install
    running build
    running build_py
    creating build
    creating build\lib.win-amd64-3.8
    creating build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\color_from_image.py -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\tokenization.py -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\wordcloud.py -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\wordcloud_cli.py -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\_version.py -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\__init__.py -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\__main__.py -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\stopwords -> build\lib.win-amd64-3.8\wordcloud
    copying wordcloud\DroidSansMono.ttf -> build\lib.win-amd64-3.8\wordcloud
    warning: cmd_build_py: byte-compiling is disabled, skipping.
    
    UPDATING build\lib.win-amd64-3.8\wordcloud/_version.py
    set build\lib.win-amd64-3.8\wordcloud/_version.py to '1.6.0'
    running build_ext
    building 'wordcloud.query_integral_image' extension
    error: Microsoft Visual C++ 14.0 is required. Get it with "Build Tools for Visual Studio": https://visualstudio.microsoft.com/downloads/
    ----------------------------------------
ERROR: Command errored out with exit status 1: 'D:\Documents\Programming\Python\venv\Scripts\python.exe' -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'C:\\Users\\<username>\\AppData\\Local\\Temp\\pycharm-packaging\\wordcloud\\setup.py'"'"'; __file__='"'"'C:\\Users\\<username>\\AppData\\Local\\Temp\\pycharm-packaging\\wordcloud\\setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record 'C:\Users\<username>\AppData\Local\Temp\pip-record-np4ix5r5\install-record.txt' --single-version-externally-managed --compile --install-headers 'D:\Documents\Programming\Python\venv\include\site\python3.8\wordcloud' Check the logs for full command output.
```

问题是，我们已经安装过了Microsoft Visual Studio并安装了其中的.NET Framework开发工具以及MSVC - VS C++ x64/x86生成工具等，不应该出现这种情况，后来发现是环境变量缺失的问题，需要在系统环境变量Path中添加X:\Program Files (x86)\Microsoft Visual Studio\201x\<version>目录，然后在命令行中进入venv虚拟环境后输入

```
pip install wordcloud
```

显示

```
Collecting wordcloud
  Using cached wordcloud-1.6.0.tar.gz (214 kB)
Requirement already satisfied: numpy>=1.6.1 in d:\documents\programming\python\venv\lib\site-packages (from wordcloud) (1.18.1)
Requirement already satisfied: pillow in d:\documents\programming\python\venv\lib\site-packages (from wordcloud) (7.0.0)
Requirement already satisfied: matplotlib in d:\documents\programming\python\venv\lib\site-packages (from wordcloud) (3.2.0)
Requirement already satisfied: kiwisolver>=1.0.1 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (1.1.0)
Requirement already satisfied: cycler>=0.10 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (0.10.0)
Requirement already satisfied: python-dateutil>=2.1 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (2.8.1)
Requirement already satisfied: pyparsing!=2.0.4,!=2.1.2,!=2.1.6,>=2.0.1 in d:\documents\programming\python\venv\lib\site-packages (from matplotlib->wordcloud) (2.4.6)
Requirement already satisfied: setuptools in d:\documents\programming\python\venv\lib\site-packages (from kiwisolver>=1.0.1->matplotlib->wordcloud) (46.0.0)
Requirement already satisfied: six in c:\users\<username>\appdata\roaming\python\python38\site-packages (from cycler>=0.10->matplotlib->wordcloud) (1.14.0)
Installing collected packages: wordcloud
    Running setup.py install for wordcloud ... done
Successfully installed wordcloud-1.6.0
```

成功安装。