---
title: Jetson TX2 Python PyTorch Illegal Hardware Instruction (Core Dumped) 解决方法
categories: SuperUser
tags:
  - python
  - pytorch
  - openblas
  - jetson
  - jetson tx2
date: 2021-12-17 15:33:36
---

该问题在网上多数帖子认为是 NumPy 的问题，但是无论是安装 numpy==1.19.4（参考[Illegal instruction (core dumped) on import for numpy 1.19.5 on ARM64 · Issue #18131 · numpy/numpy (github.com)](https://github.com/numpy/numpy/issues/18131)）

```bash
pip3 install numpy==1.19.
```


还是编译安装 numpy==1.19.5

```bash
pip install --no-binary :all: numpy==1.19.5
```

还是参考[Illegal instruction (core dumped) - Jetson & Embedded Systems / Jetson TX2 - NVIDIA Developer Forums](https://forums.developer.nvidia.com/t/illegal-instruction-core-dumped/165488/15)先全部卸载

```bash
pip3 uninstall numpy && sudo pip3 uninstall numpy
```

再安装还是添加环境变量

```bash
export OPENBLAS_CORETYPE=ARMV8
```

或

```bash
export OPENBLAS_CORETYPE=AARCH64
```

都没有解决问题。最后试图卸载之前使用 pip 安装的 torch，然后安装[PyTorch for Jetson - version 1.10 now available - Jetson Nano - NVIDIA Developer Forums](https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-10-now-available/72048?u=yihuajack) 里 Nvidia 官方提供的 torch wheel：https://nvidia.box.com/shared/static/fjtbno0vpo676a25cgvuqc1wty0fkkg6.whl 安装。安装后可能报错

> ImportError: libopenblas.so.0: cannot open shared object file: No such file or directory

参考[PyTorch for Jetson - version 1.10 now available - Jetson & Embedded Systems / Jetson Nano - NVIDIA Developer Forums](https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-10-now-available/72048/384)通过

```bash
find ~ -name libopenblas*
```

或

```bash
sudo apt install mlocate
sudo updatedb
locate libopenblas
```

寻找 libopenblas 的位置，如果未安装，则安装

```bash
sudo apt install libopenblas-dev
```

需要约 13.7 MB 存档、153 MB 空间。我的 Jetson TX2 之前已安装 OpenMPI，所以有 libmpi，如果没有则需要安装。安装完成后问题即解决。
