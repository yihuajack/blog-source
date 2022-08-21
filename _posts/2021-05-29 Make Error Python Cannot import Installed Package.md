---
title: Make报错Python无法import已安装的包的一种原因
categories: SuperUser
tags:
  - cmake
  - make
  - python
  - wsl
date: 2021-05-29 02:10:48
---

在编译DREAMPlace时执行

```bash
sudo make
```

结果显示

> Traceback (most recent call last):
>   File "/mnt/d/Documents/Junior/Research_Intern_Program/DREAMPlace/build/dreamplace/ops/dct/setup.py", line 11, in <module>
>     import torch
> ModuleNotFoundError: No module named 'torch'
> make[2]: \*** [dreamplace/ops/dct/CMakeFiles/dct.dir/build.make:76: dreamplace/ops/dct/dct.stamp] Error 1
> make[1]: \*** [CMakeFiles/Makefile2:3034: dreamplace/ops/dct/CMakeFiles/dct.dir/all] Error 2
> make: *** [Makefile:130: all] Error 2 

查看dreamplace/ops/dct/CMakeFiles/dct.dir/build.make的第76行为

```makefile
cd /mnt/d/Documents/Junior/Research_Intern_Program/DREAMPlace/build/dreamplace/ops/dct && /usr/bin/python /mnt/d/Documents/Junior/Research_Intern_Program/DREAMPlace/build/dreamplace/ops/dct/setup.py build --build-temp=/mnt/d/Documents/Junior/Research_Intern_Program/DREAMPlace/build/dreamplace/ops/dct/build --build-lib=/mnt/d/Documents/Junior/Research_Intern_Program/DREAMPlace/build/dreamplace/ops/dct/lib
```

测试命令

```bash
python
```

和

```bash
python3
```

都可以import，但是

```bash
/usr/bin/python
```

无法import，得知原因是在~.zshrc中使用了

```bash
alias python='/usr/bin/python3.8'
```

于是将该行注释掉，转而执行命令

```bash
sudo rm /usr/bin/python
sudo ln -s /usr/bin/python3.8 /usr/bin/python
```

再试，结果仍然是报相同的错。手动执行/usr/bin/python命令能够成功执行，却不能make。最后发现是使用sudo的原因，改为执行

```bash
make
```

即成功make。
