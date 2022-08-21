---
title: NVIDIA Jetson TX2安装TensorFlow
categories: SuperUser
tags:
  - tensorflow
  - python
  - jetson
  - jetson tx2
  - nvidia
date: 2021-11-09 23:42:12
---

本文基于官方文档
Installing TensorFlow For Jetson Platform :: NVIDIA Deep Learning Frameworks Documentation
https://docs.nvidia.com/deeplearning/frameworks/install-tf-jetson-platform/index.html
编译安装 TensorFlow 1 （1.15.2）+ GPU 方法另请参考
AArch64编译安装特定版本TensorFlow及Bazel_yihuajack的博客-CSDN博客
ALBERT 的 requirements.txt 要求 tensorflow==1.15.2，而这显然是不能通过 Ubuntu apt 安装的，也根本没有为 aarch64 架构编译好的 binary，所以采用编译安装。首先在 tensorflow 的 GitHub Release 中找到 1.15.2 版本，下载 Assets 中的 Source code (tar.gz)，然后解压。这时如果直接执行 sudo ./configure 会报错找不到 bazel。参考最新版本的 bazelbuild
https://blog.csdn.net/yihuajack/article/details/121045347?spm=1001.2014.3001.5501

编译安装 TensorFlow 2 （2.7）+ GPU 方法另请参考

Linux AArch64编译安装Bazel及TensorFlow 2 GPU支持_yihuajack的博客-CSDN博客
本文基于AArch64编译安装特定版本TensorFlow及Bazel_yihuajack的博客-CSDN博客https://blog.csdn.net/yihuajack/article/details/121045347注意：编译必须要求有能连接 GitHub、Google API、Google Storage 等资源的能力。设置好上网环境后，参考Installing Bazel using Bazelisk - Bazel main 直接使用 bazelisk 安装 Bazel （当前版本为 3.
https://blog.csdn.net/yihuajack/article/details/121213409?spm=1001.2014.3001.5501
首先必须要有 JetPack 环境并配置好 APT 源、网络等。执行

```bash
sudo apt update
sudo apt install libhdf5-serial-dev hdf5-tools libhdf5-dev zlib1g-dev zip libjpeg8-dev liblapack-dev libblas-dev gfortran
sudo apt install python3-pip
sudo pip3 install -U pip testresources setuptools==49.6.0
sudo pip3 install -U --no-deps numpy==1.19.4 future==0.18.2 mock==3.0.5 keras_preprocessing==1.1.2 keras_applications==1.0.8 gast==0.4.0 protobuf pybind11 cython pkgconfig
sudo env H5PY_SETUP_REQUIRES=0 pip3 install -U h5py==3.1.0
sudo pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v46 tensorflow
sudo apt install virtualenv
python3 -m virtualenv -p python3 <chosen_venv_name>
source <chosen_venv_name>/bin/activate
pip3 install -U numpy grpcio absl-py py-cpuinfo psutil portpicker six mock requests gast h5py astor termcolor protobuf keras-applications keras-preprocessing wrapt google-pasta setuptools testresources
```

对于 tensorflow 2.6.0+nv21.9，要求的 dependency 版本为：

> absl-py\==0.12.0
> gast==0.4.0
> six~=1.15.0
> wrapt~=1.12.1

如果 import tensorflow 报错

> [1]    8707 illegal hardware instruction (core dumped)  python3

参考 [Illegal instruction (core dumped) on import for numpy 1.19.5 on ARM64 · Issue #18131 · numpy/numpy (github.com)](https://github.com/numpy/numpy/issues/18131) 执行

```bash
sudo pip3 install numpy==1.19.4
```

或

```bash
sudo pip3 install --no-binary :all: numpy==1.19.5
```

安装 Anaconda 3

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-aarch64.sh
sudo bash ~/Anaconda3-2021.05-Linux-aarch64.sh
```

但是执行 conda 命令（conda info，conda list）会报错

> [1]    11034 illegal hardware instruction (core dumped)  conda info

参考 [libcrypto on ARMv8 (Rpi4) fails with Illegal Instruction · Issue #12443 · ContinuumIO/anaconda-issues (github.com)](https://github.com/ContinuumIO/anaconda-issues/issues/12443) 和 [Conda on Nvidia Jetson tx1 tx2? · Issue #1587 · ContinuumIO/anaconda-issues (github.com)](https://github.com/ContinuumIO/anaconda-issues/issues/1587)，这是由于 Anaconda 目前官方给出的 AArch64 安装脚本是基于 Neoverse N1/N2 微架构的，是用 -march=armv8.2-a+fp16+rcpc+dotprod+crypto 选项编译的，而 ARM CPU 有很多种，所以会有非法指令。

在虚拟环境中安装 h5py 时报错

> Using cached h5py-3.1.0.tar.gz (371 kB)
> Installing build dependencies ... done
> Getting requirements to build wheel ... done
> Installing backend dependencies ... error
> ERROR: Command errored with exit status 1:
> comand: /home/yihua/tf1/bin/python /home/yihua/tf1/lib/python3.6/site-packages/pip install --ignore-installed --no-user -prefix /tmp/pip-build-env-dpuj0w44/normal --no-warn-script-location --no-binary :none: --only-binary :none: -i https://pypi.org/simple -- pkgconfig 'numpy\==1.14.5; python_version == "3.7"' 'numpy\==1.17.5; python_version == "3.8"' 'numpy\==1.19.3; python_version >= "3.9"' 'Cython>=0.29; python_version < "3.8"' 'numpy\==1.12; python_version == "3.6"' 'Cython>=0.29.14; python_version >= "3.8"'
> cwd: None
> Complete output (2509 lines):
> Ignoring numpy: markers 'python_version == "3.7"' don't match your environment
> Ignoring numpy: markers 'python_version == "3.8"' don't match your environment
> Ignoring numpy: markers 'python_version >= "3.9"' don't match your environment
> Ignoring Cython: markers 'python_version == "3.8"' don't match your environment
> Collecting pkgconfig
> Using cached pkgconfig-1.5.5-py3-none-any.whl (6.7 kB)
> Collecting Cython>=0.29
> Using cached Cython-0.29.24-cp36-cp36m-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (1.9 MB)
> Collecting numpy==1.12
> Using cached numpy-1.12.0.zip (4.8 MB)
> Preparing metadata (setup.py): started
> Preparing metadata (setup.py): finished with status ‘done’
> Building wheels for collected packages: numpy
> Building wheel for numpy (setup.py): started
> Building wheel for numpy (setup.py): still running...
> Building wheel for numpy (setup.py): still running...
> Building wheel for numpy (setup.py): finished with status 'error'
> ERROR: Command errored out with exit status 1:
>
> ...
>
> ERROR: Command errored out with exit status 1
>
> ...
>
> WARNING: Discarding ...
> Using cached h5py-3.0.0.tar.gz (370 kB)
> Installing build dependencies ... done
> Getting requirements to build wheel ... done
> Installing backend dependencies ... error
> ERROR: Command errored out with exit status 1:
> comand: /home/yihua/tf1/bin/python /home/yihua/tf1/lib/python3.6/site-packages/pip install --ignore-installed --no-user -prefix /tmp/pip-build-env-dpuj0w44/normal --no-warn-script-location --no-binary :none: --only-binary :none: -i https://pypi.org/simple -- pkgconfig 'numpy\==1.14.5; python_version == "3.7"' 'numpy\==1.17.5; python_version == "3.8"' 'numpy\==1.19.3; python_version >= "3.9"' 'Cython>=0.29; python_version < "3.8"' 'numpy\==1.12; python_version == "3.6"' 'Cython>=0.29.14; python_version >= "3.8"'
> cwd: None
> Complete output (2509 lines):
> Ignoring numpy: markers 'python_version == "3.7"' don't match your environment
> Ignoring numpy: markers 'python_version == "3.8"' don't match your environment
> Ignoring numpy: markers 'python_version >= "3.9"' don't match your environment
> Ignoring Cython: markers 'python_version == "3.8"' don't match your environment
> Collecting pkgconfig
> Using cached pkgconfig-1.5.5-py3-none-any.whl (6.7 kB)
> Collecting Cython>=0.29
> Using cached Cython-0.29.24-cp36-cp36m-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (1.9 MB)
> Collecting numpy==1.12
> Using cached numpy-1.12.0.zip (4.8 MB)
> Preparing metadata (setup.py): started
> Preparing metadata (setup.py): finished with status ‘done’
> Building wheels for collected packages: numpy
> Building wheel for numpy (setup.py): started
> Building wheel for numpy (setup.py): still running...
> Building wheel for numpy (setup.py): still running...
> Building wheel for numpy (setup.py): finished with status 'error'
> ERROR: Command errored out with exit status 1:
>
> ...

虚拟环境中不能使用 sudo，env 命令也会失效，在 build wheel 的时候会提示 ModuleNotFoundError: No Module Named 'Cython'，即使你已经安装了 Cython。如果在主环境中不加 env 就会产生同意的错误。参考了 [Cannot install h5py on Jetson Xavier NX - Jetson & Embedded Systems / Jetson Xavier NX - NVIDIA Developer Forums](https://forums.developer.nvidia.com/t/cannot-install-h5py-on-jetson-xavier-nx/129261/3)、[Can't install h5py on Jetpack 4.3 - Jetson & Embedded Systems / Jetson TX2 - NVIDIA Developer Forums](https://forums.developer.nvidia.com/t/cant-install-h5py-on-jetpack-4-3/144272/9)、[cython: compilation error while building h5py (solved by workaround) · Issue #1533 · h5py/h5py (github.com)](https://github.com/h5py/h5py/issues/1533)

都不会解决该问题。最后想到一个办法，因为在主环境中是可以正常用 sudo env 编译安装 h5py 的，所以参考 [How to use Python's pip to download and keep the zipped files for a package? - Stack Overflow](https://stackoverflow.com/questions/7300321/how-to-use-pythons-pip-to-download-and-keep-the-zipped-files-for-a-package) 在主环境中执行

```bash
sudo env H5PY_SETUP_REQUIRES=0 pip3 download -d /ssddata/h5py h5py==3.1.0
```

会在 /ssddata/h5py 目录下下载生成 cached_property-1.5.2-py2.py3-none-any.whl、h5py-3.1.0.tar.gz、numpy-1.19.5-cp36-cp36m-manylinux2014_aarch64.whl 三个文件。h5py-3.1.0是最后一个支持 Python 3.6 的版本。然后执行

```bash
sudo env H5PY_SETUP_REQUIRES=0 pip3 wheel -w /ssddata/h5py h5py=3.1.0
```

会在 /ssddata/h5py 目录下生成 h5py-3.1.0-cp36-cp36m-linux_aarch64.whl。我已在 GitHub 上发布了我编译好的版本：[GitHub - yihuajack/h5py-aarch64: h5py wheel for AArch64 architecture](https://github.com/yihuajack/h5py-aarch64)。然后切换到虚拟环境中执行

```bash
pip3 install /ssddata/h5py/h5py-3.1.0-cp36-cp36m-linux_aarch64.whl
pip3 install -U numpy grpcio absl-py py-cpuinfo psutil portpicker six mock requests gast astor termcolor protobuf keras-applications keras-preprocessing wrapt google-pasta setuptools testresources
pip3 install --pre --extra-index-url https://developer.download.nvidia.com/compute/redist/jp/v46 'tensorflow<2'
```

可以自行到 extra-index-url 的 URL 地址查看包含哪些版本的 wheel。在安装 tensorflow<2 时它还会自动下载编译安装 numpy-1.18.5 并试图编译 h5py 并报错

> running build_ext
> ERROR: Failed building wheel for h5py
> Running setup.py clean for h5py

然后生成 numpy 的 wheel，提示 numpy 构建成功、h5py 构建失败，然后安装一系列包，其中要卸载 numpy-1.19.5、h5py-3.1.0（Running setup.py install for h5py ... done）、gast-0.5.2。最后安装的包有：

> astunparse-1.6.3 dataclasses-0.8 gast-0.3.3 h5py-2.10.0
> importlib-metadata-4.8.2 markdown-3.3.4 numpy-1.18.5 opt-einsum-3.3.0
> tensorboard-1.15.0 tensorflow-1.15.5+nv21.9 tensorflow-estimator-1.15.1
> typing-extensions-3.10.0.2 werkzeug-2.0.2 zipp-3.6.0

补充：由于 Jetson TX2 自带实际可用磁盘空间仅为 28768392 个 1K-blocks（29.5 GB），其中还有超过 1% 必须为系统所保留，否则启动时会崩溃，所以需要在外接硬盘上创建虚拟环境。默认挂载的权限是 rw，也就是读写，而没有 x 也就是执行的权限，创建虚拟环境会报错

> virtualenv: error: argument dest: the destination . is not write-able at /ssddata
> SystemExit: 2

如果之前已在本地磁盘创建好虚拟环境，可参考 [linux上python虚拟环境迁移方法 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/134306869) 进行迁移，也可以参考 [How do I set read/write permissions my hard drives? - Ask Ubuntu](https://askubuntu.com/questions/90339/how-do-i-set-read-write-permissions-my-hard-drives) 更改挂载的目录的权限或用 rwx 权限重新 mount，然后直接在挂载的文件目录下重新新建虚拟环境。
