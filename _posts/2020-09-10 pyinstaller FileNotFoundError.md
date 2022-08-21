---
title: 'pyinstaller报错FileNotFoundError：[WinError 3] 系统找不到指定的路径的解决方法'
categories: CSDN补档
tags:
  - python
  - pyinstaller
  - venv
abbrlink: 21491
date: 2020-09-10 17:22:04
---

进入python venv环境，返回python项目所在路径，执行

```bash
pyinstaller setup.py
```

或

```bash
pyinstaller -F setup.py
```

给出以下信息：（不同环境运行会略有不同）

```
331 INFO: PyInstaller: 4.0
333 INFO: Python: 3.8.5
334 INFO: Platform: Windows-10-10.0.19041-SP0
360 INFO: wrote D:\Documents\Programming\Python\Canvas-Syncer-Test\setup.spec
396 INFO: UPX is not available.
401 INFO: Extending PYTHONPATH with paths
['D:\\Documents\\Programming\\Python\\Canvas-Syncer-Test',
 'D:\\Documents\\Programming\\Python\\Canvas-Syncer-Test']
452 INFO: checking Analysis
455 INFO: Building Analysis because Analysis-00.toc is non existent
456 INFO: Initializing module dependency graph...
475 INFO: Caching module graph hooks...
526 INFO: Analyzing base_library.zip ...
5147 INFO: Processing pre-find module path hook distutils from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks\\pre_fin
d_module_path\\hook-distutils.py'.
5152 INFO: distutils: retargeting to non-venv dir 'D:\\Program_Files\\msys64\\mingw64\\lib\\python3.8'
9536 INFO: Caching module dependency graph...
9765 INFO: running Analysis Analysis-00.toc
9769 INFO: Adding Microsoft.Windows.Common-Controls to dependent assemblies of final executable
  required by d:\program_files\venv\bin\python3.exe
9879 INFO: Analyzing D:\Documents\Programming\Python\Canvas-Syncer-Test\setup.py
10262 INFO: Processing pre-find module path hook site from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks\\pre_find_mo
dule_path\\hook-site.py'.
10264 INFO: site: retargeting to fake-dir 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\fake-modules'
11513 INFO: Processing pre-safe import module hook setuptools.extern.six.moves from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInst
aller\\hooks\\pre_safe_import_module\\hook-setuptools.extern.six.moves.py'.
12981 INFO: Processing module hooks...
12985 INFO: Loading module hook 'hook-distutils.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
12994 INFO: Loading module hook 'hook-encodings.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
13438 INFO: Loading module hook 'hook-lib2to3.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
14037 INFO: Loading module hook 'hook-pkg_resources.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
15096 INFO: Processing pre-safe import module hook win32com from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\_pyinstaller_hooks_contri
b\\hooks\\pre_safe_import_module\\hook-win32com.py'.
Traceback (most recent call last):
  File "<string>", line 2, in <module>
ModuleNotFoundError: No module named 'win32com'
15216 INFO: Processing pre-safe import module hook win32com from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\_pyinstaller_hooks_contri
b\\hooks\\pre_safe_import_module\\hook-win32com.py'.
Traceback (most recent call last):
  File "<string>", line 2, in <module>
ModuleNotFoundError: No module named 'win32com'
15407 WARNING: Hidden import "pkg_resources.markers" not found!
15410 INFO: Excluding import '__main__'
15416 INFO:   Removing import of __main__ from module pkg_resources
15422 INFO: Loading module hook 'hook-setuptools.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
16977 INFO: Loading module hook 'hook-sysconfig.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
16981 INFO: Loading module hook 'hook-xml.dom.domreg.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
16984 INFO: Loading module hook 'hook-xml.etree.cElementTree.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'.
..
16986 INFO: Loading module hook 'hook-xml.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
16996 INFO: Loading module hook 'hook-_tkinter.py' from 'D:\\program_files\\venv\\lib\\python3.8\\site-packages\\PyInstaller\\hooks'...
Traceback (most recent call last):
  File "D:\Program_Files\msys64\mingw64\lib\python3.8\runpy.py", line 194, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "D:\Program_Files\msys64\mingw64\lib\python3.8\runpy.py", line 87, in _run_code
    exec(code, run_globals)
  File "D:\Program_Files\venv\bin\pyinstaller.exe\__main__.py", line 7, in <module>
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\__main__.py", line 114, in run
    run_build(pyi_config, spec_file, **vars(args))
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\__main__.py", line 65, in run_build
    PyInstaller.building.build_main.main(pyi_config, spec_file, **kwargs)
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\building\build_main.py", line 720, in main
    build(specfile, kw.get('distpath'), kw.get('workpath'), kw.get('clean_build'))
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\building\build_main.py", line 667, in build
    exec(code, spec_namespace)
  File "D:\Documents\Programming\Python\Canvas-Syncer-Test\setup.spec", line 6, in <module>
    a = Analysis(['setup.py'],
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\building\build_main.py", line 242, in __init__
    self.__postinit__()
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\building\datastruct.py", line 160, in __postinit__
    self.assemble()
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\building\build_main.py", line 419, in assemble
    self.graph.process_post_graph_hooks()
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\depend\analysis.py", line 365, in process_post_graph_hooks
    module_hook.post_graph()
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\depend\imphook.py", line 444, in post_graph
    self._process_hook_func()
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\depend\imphook.py", line 463, in _process_hook_func
    self._hook_module.hook(hook_api)
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\hooks\hook-_tkinter.py", line 251, in hook
    hook_api.add_datas(_collect_tcl_tk_files(hook_api))
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\hooks\hook-_tkinter.py", line 212, in _collect_tcl_tk_files
    _handle_broken_tcl_tk()
  File "D:\program_files\venv\lib\python3.8\site-packages\PyInstaller\hooks\hook-_tkinter.py", line 42, in _handle_broken_tcl_tk
    files = os.listdir(basedir)
FileNotFoundError: [WinError 3] 系统找不到指定的路径。: 'D:\\Program_Files\\msys64\\mingw64\\tcl'
```

tcl文件夹原本应处于Python38文件夹下，可见是路径错误。即使将D:\Program_Files\msys64\mingw64\bin路径从环境变量中移除后重启后问题依然存在，或者什么都不显示/执行。克隆pyinstaller库，找到PyInstaller\hooks\hook-_tkinter.py第41-42行：

```
basedir = os.path.join(base_prefix, 'tcl')
files = os.listdir(basedir)
```

此时将listdir将makedirs显然不合要求，发现base_prefix是从compat里import的，于是在PyInstaller\compat.py中找到第149-151行：

```
base_prefix = os.path.abspath(
    getattr(sys, 'real_prefix', getattr(sys, 'base_prefix', sys.prefix))
)
```

可见其正确确认了venv的运行环境（可参考[确定Python是否在virtualenv中运行](https://www.codenong.com/1871549/)），加之发现不管如何启动venv都有错误符号，于是根据另一篇文章[Python3 venv没反应或错误的原因](https://blog.csdn.net/yihuajack/article/details/108518017)重新生成并激活了venv，安装最新development版本：

```bash
pip install https://github.com/pyinstaller/pyinstaller/archive/develop.zip
```

然后再执行pyinstaller命令，成功生成：

```
72 INFO: PyInstaller: 4.1.dev0
72 INFO: Python: 3.8.5
73 INFO: Platform: Windows-10-10.0.19041-SP0
74 INFO: wrote D:\Documents\Programming\Python\Canvas-Syncer-Test\setup.spec
80 INFO: UPX is not available.
81 INFO: Extending PYTHONPATH with paths
['D:\\Documents\\Programming\\Python\\Canvas-Syncer-Test',
 'D:\\Documents\\Programming\\Python\\Canvas-Syncer-Test']
88 INFO: checking Analysis
88 INFO: Building Analysis because Analysis-00.toc is non existent
89 INFO: Initializing module dependency graph...
91 INFO: Caching module graph hooks...
104 INFO: Analyzing base_library.zip ...
2120 INFO: Processing pre-find module path hook distutils from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks\\pre_find_module_path\\hook-distutils.py'.
2121 INFO: distutils: retargeting to non-venv dir 'D:\\Program_Files\\Python38\\lib'
4015 INFO: Caching module dependency graph...
4120 INFO: running Analysis Analysis-00.toc
4126 INFO: Adding Microsoft.Windows.Common-Controls to dependent assemblies of final executable
  required by d:\program_files\venv\scripts\python.exe
4138 INFO: Analyzing D:\Documents\Programming\Python\Canvas-Syncer-Test\setup.py
4331 INFO: Processing pre-find module path hook site from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks\\pre_find_module_path\\hook-site.py'.
4332 INFO: site: retargeting to fake-dir 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\fake-modules'
5043 INFO: Processing pre-safe import module hook setuptools.extern.six.moves from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks\\pre_safe_import_module\\hook-setuptools.extern.six.moves.py'.
5569 INFO: Processing module hooks...
5570 INFO: Loading module hook 'hook-difflib.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5571 INFO: Excluding import 'doctest'
5572 INFO:   Removing import of doctest from module difflib
5572 INFO: Loading module hook 'hook-distutils.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5574 INFO: Loading module hook 'hook-distutils.util.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5575 INFO: Excluding import 'lib2to3.refactor'
5576 INFO:   Removing import of lib2to3.refactor from module distutils.util
5576 INFO: Loading module hook 'hook-encodings.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5671 INFO: Loading module hook 'hook-heapq.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5672 INFO: Excluding import 'doctest'
5673 INFO:   Removing import of doctest from module heapq
5673 INFO: Loading module hook 'hook-lib2to3.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5838 INFO: Loading module hook 'hook-multiprocessing.util.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5839 INFO: Excluding import 'test'
5840 INFO:   Removing import of test.support from module multiprocessing.util
5840 INFO:   Removing import of test from module multiprocessing.util
5840 INFO: Loading module hook 'hook-pickle.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
5841 INFO: Excluding import 'argparse'
5842 INFO:   Removing import of argparse from module pickle
5842 INFO: Loading module hook 'hook-pkg_resources.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
6260 INFO: Processing pre-safe import module hook win32com from 'd:\\program_files\\venv\\lib\\site-packages\\_pyinstaller_hooks_contrib\\hooks\\pre_safe_import_module\\hook-win32com.py'.
Traceback (most recent call last):
  File "<string>", line 2, in <module>
ModuleNotFoundError: No module named 'win32com'
6326 INFO: Processing pre-safe import module hook win32com from 'd:\\program_files\\venv\\lib\\site-packages\\_pyinstaller_hooks_contrib\\hooks\\pre_safe_import_module\\hook-win32com.py'.
Traceback (most recent call last):
  File "<string>", line 2, in <module>
ModuleNotFoundError: No module named 'win32com'
6395 WARNING: Hidden import "pkg_resources.markers" not found!
6396 INFO: Excluding import '__main__'
6397 INFO:   Removing import of __main__ from module pkg_resources
6397 INFO: Loading module hook 'hook-setuptools.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
6924 INFO: Excluding import 'setuptools.py33compat'
6925 INFO:   Removing import of setuptools.py33compat from module setuptools.package_index
6925 INFO:   Removing import of setuptools.py33compat from module setuptools.depends
6926 INFO: Excluding import 'setuptools.py27compat'
6926 INFO:   Removing import of setuptools.py27compat from module setuptools.command.easy_install
6927 INFO:   Removing import of setuptools.py27compat from module setuptools.package_index
6927 INFO:   Removing import of setuptools.py27compat from module setuptools.depends
6928 INFO: Loading module hook 'hook-sysconfig.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
6929 INFO: Loading module hook 'hook-xml.dom.domreg.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
6929 INFO: Loading module hook 'hook-xml.etree.cElementTree.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
6930 INFO: Loading module hook 'hook-xml.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
6930 INFO: Loading module hook 'hook-_tkinter.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
7179 INFO: checking Tree
7179 INFO: Building Tree because Tree-00.toc is non existent
7179 INFO: Building Tree Tree-00.toc
7319 INFO: checking Tree
7320 INFO: Building Tree because Tree-01.toc is non existent
7320 INFO: Building Tree Tree-01.toc
7338 INFO: Loading module hook 'hook-setuptools.msvc.py' from 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks'...
7340 INFO: Excluding import 'numpy'
7340 INFO:   Removing import of numpy from module setuptools.msvc
7352 INFO: Looking for ctypes DLLs
7392 INFO: Analyzing run-time hooks ...
7395 INFO: Including run-time hook 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks\\rthooks\\pyi_rth_multiprocessing.py'
7398 INFO: Including run-time hook 'd:\\program_files\\venv\\lib\\site-packages\\PyInstaller\\hooks\\rthooks\\pyi_rth_pkgres.py'
7404 INFO: Looking for dynamic libraries
7961 INFO: Looking for eggs
7962 INFO: Using Python library D:\Program_Files\Python38\python38.dll
7962 INFO: Found binding redirects:
[]
7965 INFO: Warnings written to D:\Documents\Programming\Python\Canvas-Syncer-Test\build\setup\warn-setup.txt
8011 INFO: Graph cross-reference written to D:\Documents\Programming\Python\Canvas-Syncer-Test\build\setup\xref-setup.html
8031 INFO: checking PYZ
8031 INFO: Building PYZ because PYZ-00.toc is non existent
8031 INFO: Building PYZ (ZlibArchive) D:\Documents\Programming\Python\Canvas-Syncer-Test\build\setup\PYZ-00.pyz
8648 INFO: Building PYZ (ZlibArchive) D:\Documents\Programming\Python\Canvas-Syncer-Test\build\setup\PYZ-00.pyz completed successfully.
8656 INFO: checking PKG
8657 INFO: Building PKG because PKG-00.toc is non existent
8657 INFO: Building PKG (CArchive) PKG-00.pkg
8672 INFO: Building PKG (CArchive) PKG-00.pkg completed successfully.
8673 INFO: Bootloader d:\program_files\venv\lib\site-packages\PyInstaller\bootloader\Windows-64bit\run.exe
8673 INFO: checking EXE
8674 INFO: Building EXE because EXE-00.toc is non existent
8674 INFO: Building EXE from EXE-00.toc
8674 INFO: Appending archive to EXE D:\Documents\Programming\Python\Canvas-Syncer-Test\build\setup\setup.exe
8677 INFO: Building EXE from EXE-00.toc completed successfully.
8680 INFO: checking COLLECT
8681 INFO: Building COLLECT because COLLECT-00.toc is non existent
8682 INFO: Building COLLECT COLLECT-00.toc
9016 INFO: Building COLLECT COLLECT-00.toc completed successfully.
```
