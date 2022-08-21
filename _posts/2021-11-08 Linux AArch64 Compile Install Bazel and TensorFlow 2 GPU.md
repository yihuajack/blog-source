---
title: Linux AArch64编译安装Bazel及TensorFlow 2 GPU支持
categories: SuperUser
tags:
  - tensorflow
  - 人工智能
  - python
  - bazel
  - aarch64
date: 2021-11-08 18:59:42
---

本文基于
AArch64编译安装特定版本TensorFlow及Bazel_yihuajack的博客-CSDN博客
https://blog.csdn.net/yihuajack/article/details/121045347

注意：编译必须要求有能连接 GitHub、Google API、Google Storage 等资源的能力。设置好上网环境后，参考 [Installing Bazel using Bazelisk - Bazel main](https://docs.bazel.build/versions/main/install-bazelisk.html) 直接使用 bazelisk 安装 Bazel （当前版本为 3.7.2）。请尽量确保环境与 [从源代码构建  | TensorFlow](https://www.tensorflow.org/install/source) “经过测试的构建配置” 一致，如果不一致，尽量在 Issue 中找到是否有人采用了同样的编译环境。如果需要更改 GCC 版本，需要执行

```bash
sudo add-apt-repository ppa:jonathonf/gcc
sudo apt-get update
sudo apt install gcc-7
```

具体包含版本可见 [GCC : Jonathon F](https://launchpad.net/~jonathonf/+archive/ubuntu/gcc)。如果第一步出错报错 No such file or directory，参考 [No such file or directory: '/etc/apt/sources.list.d/webupd8team-ubuntu-java-bionic.list'](https://askubuntu.com/questions/1106668/no-such-file-or-directory-etc-apt-sources-list-d-webupd8team-ubuntu-java-bion)，这可能是由于先前清理掉了 sources.list.d 目录，执行

```bash
sudo mkdir /etc/apt/sources.list.d
```

```bash
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
git checkout r2.7
sudo .configure
sudo -E bazel --output_user_root=/ssddata/bazel build --config=dbg --config=cuda //tensorflow/tools/pip_package:build_pip_package
```

Configure 时如在 Do you wish to download a fresh release of clang? (Expermental) [y/N] 选择了 y 的话需要在 .rc 文件中指定 Config value 'download_clang_use_lld'。不推荐使用 Clang，因为“经过测试的构建配置”的 Linux 版本全部是使用 GCC 构建的。在 Bazel 构建过程中，会先寻找可用的配置定义，然后分析 target（加载 452 个包，配置 28735 个 target），然后编译 target，这一步需要消耗大量的系统内存资源，像 Jetson TX2 的 8 GiB 内存是不够用的。完成后，执行

```bash
sudo -E bazel build --config=v1 --config=cuda //tensorflow/tools/pip_package:build_pip_package
/bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
pip install ~/tensorflow_pkg/tensorflow-1.15.2-cp36-cp36m-linux_aarch64.whl
```

构建 whl 包。
