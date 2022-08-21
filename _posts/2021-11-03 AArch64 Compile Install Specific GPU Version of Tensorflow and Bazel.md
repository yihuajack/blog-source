---
title: AArch64编译安装特定GPU版本TensorFlow及Bazel
categories: SuperUser
tags:
  - tensorflow
  - ubuntu
  - 人工智能
  - aarch64
  - bazel
date: 2021-11-03 02:02:21
---

前排提示：如果使用的 cuDNN 版本高于 7，会无法编译安装带 CUDA 支持的 TensorFlow 1 版本。

本文基于 Jetson TX2。

ALBERT 的 requirements.txt 要求 tensorflow==1.15.2，而这显然是不能通过 Ubuntu apt 安装的，也根本没有为 aarch64 架构编译好的 binary，所以采用编译安装。

首先在 tensorflow 的 GitHub Release 中找到 1.15.2 版本，下载 Assets 中的 Source code (tar.gz)，然后解压。这时如果直接执行 sudo ./configure 会报错找不到 bazel。参考最新版本的 bazelbuild 的文档：[Installing Bazel on Ubuntu - Bazel 4.2.1](https://docs.bazel.build/versions/4.2.1/install-ubuntu.html)：

一、使用 Bazelisk 安装。第一种方法使用

```bash
npm install -g @bazel/bazelisk
```

可以成功安装 Bezelisk；第二种方法直接下载 GitHub Release；第三种方法要求 MacOS 的 homebrew，不适用；第四种方法

```bash
go get github.com/bazelbuild/bazelisk
```

要求安装 Go，在下载了最新版 Go 的 Linux aarch64 版本的 binary 后移动到 /usr/local 目录下并添加至 $PATH，安装完成，但是 go 安装包很慢，所以不采用。

然而，bazelisk 默认安装的 bazel 版本过高，提示要求 0.26.1 及以下版本，无法使用。

二、使用定制的 APT 仓库

```bash
sudo apt install curl gnupg
curl -fsSL https://bazel.build/bazel-release.pub.gpg | gpg --dearmor > bazel.gpg
sudo mv bazel.gpg /etc/apt/trusted.gpg.d/
echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
sudo apt update && sudo apt install bazel=0.26.1
```

虽然能成功 update，但是安装时仍会提示找不到包，因为 googleapis.com 是无法访问的。

三、使用 binary 安装程序

同样，0.26.1 版本没有 Linux aarch64 的 binary，无法使用。

四、只能手动编译 bazel-0.26.1。你可以参考 0.26.0 版本的文档，安装指导部分的内容与最新版本差异很小：[Compiling Bazel from source - Bazel 0.26.0](https://docs.bazel.build/versions/0.26.0/install-compile-source.html)。

方法一：使用 Bazel 构建 Bazel：

从 GitHub Release 下载 bazel-0.26.1 Source code (tar.gz) 并解压，执行

```bash
bazel build //src:bazel-dev
```

类似命令都需要访问 googleapis.com，无法使用。

方法二：使用 scratch 构建 Bazel：

从 GitHub Release 下载 [bazel-0.26.1-dist.zip](https://github.com/bazelbuild/bazel/releases/download/0.26.1/bazel-0.26.1-dist.zip) （注意不是 Source code，如果直接用 Source Code 会提示需要设置 PROTOC，设置 PROTOC 后会提示需要设置 GRPC_JAVA_PLUGIN，提示出现错误的原因是在编译一个 develop checkout）并解压，执行

```bash
sudo apt-get install build-essential openjdk-11-jdk python zip unzip
```

安装编译所需依赖。切换到解压后的目录下，执行

```bash
env EXTRA_BAZEL_ARGS="--host_javabase=@local_jdk//:jdk" bash ./compile.sh
```

注意：如果你已安装 CUDA，那么需要使用 root 权限执行这条命令，然后你仍会遇到报错：

> ERROR: /home/yihua/bazel-0.26.1-dist/src/BUILD:339:2: Executing genrule
> //src:package-zip_nojdk failed (Exit 126): bash failed: error executing
> command 
>   (cd /tmp/bazel_fMnQfyqa/out/execroot/io_bazel && \
>   exec env - \
>     PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
> \
>   /bin/bash -c 'source external/bazel_tools/tools/genrule/genrule-setup.sh;
> src/package-bazel.sh bazel-out/aarch64-opt/bin/src/package_nojdk.zip
> bazel-out/aarch64-opt/bin/src/embedded_tools_nojdk.zip
> bazel-out/aarch64-opt/bin/src/main/java/com/google/devtools/build/lib/bazel/BazelServer_deploy.jar
> bazel-out/aarch64-opt/bin/src/install_base_key_nojdk
> bazel-out/aarch64-opt/bin/src/main/native/libunix.so
> bazel-out/aarch64-opt/bin/src/main/tools/build-runfiles
> bazel-out/aarch64-opt/bin/src/main/tools/process-wrapper
> src/main/tools/jdk.BUILD
> bazel-out/aarch64-opt/bin/src/main/tools/linux-sandbox
> bazel-out/aarch64-opt/bin/tools/osx/xcode-locator
> bazel-out/aarch64-opt/bin/src/main/tools/daemonize')
> Execution platform: @bazel_tools//platforms:host_platform
> /bin/bash: src/package-bazel.sh: Permission denied
> Target //src:bazel_nojdk failed to build
> INFO: Elapsed time: 1144.984s, Critical Path: 263.94s
> INFO: 1749 processes: 1514 local, 235 worker.
> FAILED: Build did NOT complete successfully
>
> ERROR: Could not build Bazel

参考 
Bazel 0.11.0 cant be built from source on Linux because of missing execute bits · Issue #4733 · bazelbuild/bazel (github.com)
https://github.com/bazelbuild/bazel/issues/4733

执行

```bash
find -name '*.sh' -exec chmod a+x '{}' \;
```

 后重新 compile。

经过较长时间的编译（Elapsed time: 1164.949s, Critical Path: 469.78s）（1751 processes: 1516 local, 235 worker）后成功编译：

> Build successful! Binary is here: /home/nvidia/Downloads/bazel-0.26.1-dist/output/bazel

将其移动至 /usr/local/bin。 

如果安装了 CUDA，bazel 需要额外编译内容，这就需要 root 权限。但使用 root 权限后仍然会报错 Permission denied 的问题，参考 [Bazel 0.11.0 cant be built from source on Linux because of missing execute bits · Issue #4733 · bazelbuild/bazel (github.com)](https://github.com/bazelbuild/bazel/issues/4733)

执行

```bash
find -name '*.sh' -exec chmod a+x '{}' \;
```

后重新 compile 即可。

回到 tensorflow-1.15.2 目录下，参考
sanjayseshan/tensorflow-aarch64: tensorflow builds for 64bit arm (github.com)
https://github.com/sanjayseshan/tensorflow-aarch64

从源代码构建  |  TensorFlow
https://www.tensorflow.org/install/source
执行

```bash
pip install -U --user pip numpy wheel
pip install -U --user keras_preprocessing --no-deps
sudo ./configure
```

在 [Home - ComputeCpp CE - Products - Codeplay Developer](https://developer.codeplay.com/products/computecpp/ce/home) 下载最新版本的 computecpp 解压 ComputeCpp-CE-2.7.0-aarch64-linux-gnu，移动到 /usr/local 并重命名为 computecpp。

参考 [How to install cuDNN on Jetson Nano? · Issue #4154 · microsoft/onnxruntime (github.com)](https://github.com/microsoft/onnxruntime/issues/4154)cuDNN 已包含在 JetPack 内，可通过在 /usr/lib/aarch64-linux-gnu 下寻找 cuDNN 版本，我的是 8.2.1。

前两步选项使用 /usr/bin/python3 和 /usr/lib/python3/dist-packages。注意不要选 OpenCL SYCL 和 ROCm，而选 CUDA，如果同时选会报错

> Traceback (most recent call last):
>   File "./configure.py", line 1602, in <module>
>     main()
>   File "./configure.py", line 1549, in main
>     raise UserInputError('SYCL / CUDA / ROCm are mututally exclusive. '
> __main__.UserInputError: SYCL / CUDA / ROCm are mututally exclusive. At most 1
> GPU platform can be configured.

在指定 CUDA compute capabilities 一步通过 [CUDA GPUs | NVIDIA Developer](https://developer.nvidia.com/cuda-gpus) 查询，对于 Jetson TX2 是 6.2。

如果报错

> Traceback (most recent call last):
>   File "configure.py", line 1571, in <module>
>     main()
>   File "configure.py", line 1448, in main
>     if validate_cuda_config(environ_cp):
>   File "configure.py", line 1340, in validate_cuda_config
>     tuple(line.decode('ascii').rstrip().split(': ')) for line in proc.stdout)
> ValueError: dictionary update sequence element #9 has length 1; 2 is required

参考 [configure.py not working on r1.14 - aarch64 nivida jetson nano · Issue #44198 · tensorflow/tensorflow (github.com)](https://github.com/tensorflow/tensorflow/issues/44198)

在 third_part/gpus/find_cuda_config.py 文件的第 355 行将 cudnn_version 手动指定为 8。

参考 [LLVM Debian/Ubuntu packages](https://apt.llvm.org/) 先将

```
deb http://apt.llvm.org/bionic/ llvm-toolchain-bionic main
deb-src http://apt.llvm.org/bionic/ llvm-toolchain-bionic main
```

添加到源，然后执行 sudo apt update，如果报错

> W: GPG error: https://apt.llvm.org/bionic llvm-toolchain-bionic InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 15CF4D18AF4F7421

执行

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 15CF4D18AF4F7421
sudo apt update
```

即可。如果报错

> Cannot initiate the connection to apt.llvm.org:80 (2a04:4e42:6::561). - connect (101: Network is unreachable) Could not connect to apt.llvm.org:80 (151.101.198.49), Connection timed out

重试即可。执行

```bash
sudo apt install clang-format clang-tidy clang-tools clang clangd libc++-dev libc++1 libc++abi-dev libc++abi1 libclang-dev libclang1 liblldb-dev libllvm-ocaml-dev libomp-dev libomp5 lld lldb llvm-dev llvm-runtime llvm python-clang
```

其中有些无法安装，从这个列表里去除。注意：apt 仓库的 aarch64 版本的 clang 已经十分老旧（6.0.0-1ubuntu2），可以选择在 https://github.com/llvm/llvm-project/releases 下载最新版本的 clang+llvm-13.0.0-aarch64-linux-gnu.tar.xz 解压编译安装。

参考 [Build from source with MPI support · Issue #25677 · tensorflow/tensorflow (github.com)](https://github.com/tensorflow/tensorflow/issues/25677) 安装 Open MPI。先从 [Open MPI: Version 4.1](https://www.open-mpi.org/software/) 下载最新版本的 *.tar.bz2 / *.tar.gz 压缩包，然后解压，参考 [FAQ: Building Open MPI (open-mpi.org)](https://www.open-mpi.org/faq/?category=building) 执行

```bash
gunzip -c openmpi-4.1.1.tar.gz | tar xf -
cd openmpi-4.1.1
sudo ./configure --prefix=/usr/local
sudo make all install
```

安装时间较长。安装成功后在 Please specify the MPI toolkit folder 时 Default 就会从 /usr 改成 /usr/local。

注意不要勾选 否则会报错

> ERROR: Config value download_clang_use_lld is not defined in any .rc file

执行完 configure 后配置上网，具体方法可参考 [软件篇-01-为Jetson TX2扫清科研的障碍 - 我叫平沢唯 - 博客园 (cnblogs.com)](https://www.cnblogs.com/QiQi-Robotics/p/13569425.html)，把网址改成正确的网址，把  fhs-install-<name> 克隆下来，然后下载最新 <name>-core 的 release 里的 <name>-linux-arm64-v8a.zip 和 <name>-linux-arm64-v8a.zip.dgst 到 fhs-install-<name> 里，将 install-release.sh 的 282-285 和 287-290 行注释掉，然后添加

```bash
cp <name>-linux-$MACHINE.zip $ZIP_FILE
cp <name>-linux-$MACHINE.zip.dgst $ZIP_FILE.dgst
```

执行

```bash
sudo bash ./install-release.sh
sudo systemctl enable <name>
sudo systemctl start <name>
sudo systemctl status <name>.service
```

显示为 Active: active (running)，也可以看到配置文件路径在 /usr/local/etc/<name>/config.json。

参考 [Download and install - The Go Programming Language (golang.org)](https://golang.org/doc/install) 下载

go1.17.3.linux-amd64.tar.gz
执行

```bash
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.17.3.linux-amd64.tar.gz
export PATH=$PATH:/usr/local/go/bin
go version
```

网络问题还可参考 [Bazel 问题 | 小蘑菇的技术之路 (yanqizhao.com)、Bazel building error : proxy address 127.0.0.1:8118 is not a valid URL · Issue #2375 · bazelbuild/bazel · GitHub](https://yanqizhao.com/2021/06/04/Bazel%20%E9%97%AE%E9%A2%98/)注意：在 Do you want to use clang as CUDA compiler? [y/N] 时选择 N，否则会报错。参考：[Illegal ambiguous match on configurable attribute "deps" in //tensorflow/core:ops · Issue #34878 · tensorflow/tensorflow (github.com)](https://github.com/tensorflow/tensorflow/issues/34878)[Illegal ambiguous match on configurable attribute "deps" in //tensorflow/tools/pip_package:included_headers_gather · Issue #24447 · tensorflow/tensorflow (github.com)](https://github.com/tensorflow/tensorflow/issues/24447)

> Auto-Configuration Warning: 'TMP' environment variable is not set, using 'C:\Windows\Temp' as default
> ERROR: /home/yihua/tensorflow-1.15.2/tensorflow/tools/pip_package/BUILD:35:1:
> Illegal ambiguous match on configurable attribute "deps" in
> //tensorflow/tools/pip_package:included_headers_gather:
> @local_config_cuda//cuda:using_nvcc
> @local_config_cuda//cuda:using_clang
> Multiple matches are not allowed unless one is unambiguously more specialized.
> ERROR: Analysis of target '//tensorflow/tools/pip_package:build_pip_package'
> failed; build aborted: 
>
> /home/yihua/tensorflow-1.15.2/tensorflow/tools/pip_package/BUILD:35:1: Illegal
> ambiguous match on configurable attribute "deps" in
> //tensorflow/tools/pip_package:included_headers_gather:
> @local_config_cuda//cuda:using_nvcc
> @local_config_cuda//cuda:using_clang
> Multiple matches are not allowed unless one is unambiguously more specialized.
> INFO: Elapsed time: 46.883s
> INFO: 0 processes.
> FAILED: Build did NOT complete successfully (41 packages loaded, 54 targets
> co\
> nfigured)
>     currently loading: tensorflow/core/kernels
>     Fetching @local_config_cc; fetching

注意：根据 TensorFlow 的版本不同可能无法兼容 cuDNN 8 和 TensorRT 8，报错：

> ERROR:
> /home/yihua/tensorflow-1.15.2/tensorflow/compiler/tf2tensorrt/BUILD:209:1: C++
> compilation of rule '//tensorflow/compiler/tf2tensorrt:trt_logging' failed
> (Exit 1)
> In file included from
> ./tensorflow/compiler/tf2tensorrt/utils/trt_logger.h:23:0,
>                  from tensorflow/compiler/tf2tensorrt/utils/trt_logger.cc:16:
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:6066:101:
> warning: 'IRNNv2Layer' is deprecated [-Wdeprecated-declarations]
>          ITensor& input, int32_t layerCount, int32_t hiddenSize, int32_t
> maxSeqLen, RNNOperation op) noexcept
>                                                                                                      ^\~\~\~\~\~\~\~
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:3086:22:
> note: declared here
>  class TRT_DEPRECATED IRNNv2Layer : public ILayer
>                       ^\~\~\~\~\~\~\~\~\~\~
> In file included from
> tensorflow/compiler/tf2tensorrt/utils/trt_logger.cc:16:0:
> ./tensorflow/compiler/tf2tensorrt/utils/trt_logger.h:32:8: error: looser throw
> specifier for 'virtual void
> tensorflow::tensorrt::Logger::log(nvinfer1::ILogger::Severity, const char\*)'
>    void log(nvinfer1::ILogger::Severity severity, const char* msg) override;
>         ^~~
> In file included from /usr/include/aarch64-linux-gnu/NvInferLegacyDims.h:53:0,
>                  from
> bazel-out/host/bin/external/local_config_tensorrt/_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:53,
>                  from ./tensorflow/compiler/tf2tensorrt/utils/trt_logger.h:23,
>                  from tensorflow/compiler/tf2tensorrt/utils/trt_logger.cc:16:
> /usr/include/aarch64-linux-gnu/NvInferRuntimeCommon.h:1222:18: error:
> overriding 'virtual void nvinfer1::ILogger::log(nvinfer1::ILogger::Severity,
> const AsciiChar\*) noexcept'
>      virtual void log(Severity severity, AsciiChar const* msg) noexcept = 0;
>                   ^~~
> Target //tensorflow/tools/pip_package:build_pip_package failed to build
> Use --verbose_failures to see the command lines of failed build steps.

参考 [fatal error: NvInfer.h: No such file or directory | TensorRT 报错处理 | 【成功解决】_墨理学AI-CSDN博客](https://blog.csdn.net/sinat_28442665/article/details/118797477)

这是由于我使用的是 TensorRT 8 的问题，参考[运行PointPillars_MultiHead_40FPS踩过的坑_Grapymage的博客-CSDN博客](https://blog.csdn.net/qq_34859576/article/details/119136957)，将 tensorflow/compiler/tf2tensorrt/utils/trt_logger.h 的第 26 行由

```c++
void Logger::log(Severity severity, const char* msg) {
```

改为

```c++
void log(Severity severity, nvinfer1::AsciiChar const* msg) noexcept {
```

重新编译，报错：

> ERROR:
> /home/yihua/tensorflow-1.15.2/tensorflow/compiler/tf2tensorrt/BUILD:34:1: C++
> compilation of rule '//tensorflow/compiler/tf2tensorrt:tensorrt_stub' failed
> (Exit 1)
> In file included from
> tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:17:0:
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:6066:101:
> warning: 'IRNNv2Layer' is deprecated [-Wdeprecated-declarations]
>          ITensor& input, int32_t layerCount, int32_t hiddenSize, int32_t
> maxSeqLen, RNNOperation op) noexcept
>                                                                                                      ^\~\~\~\~\~\~\~
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:3086:22:
> note: declared here
>  class TRT_DEPRECATED IRNNv2Layer : public ILayer
>                       ^\~\~\~\~\~\~\~\~\~\~
> In file included from
> tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:56:0:
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc: In function 'void*
> createInferBuilder_INTERNAL(void*, int)':
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc:5:7: error: declaration
> of 'void* createInferBuilder_INTERNAL(void*, int)' has a different exception
> specifier
>  void* createInferBuilder_INTERNAL(void* logger, int version) {
>        ^\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~
> In file included from
> tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:17:0:
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:8127:30:
> note: from previous declaration 'void* createInferBuilder_INTERNAL(void*,
> int32_t) noexcept'
>  extern "C" TENSORRTAPI void* createInferBuilder_INTERNAL(void* logger,
> int32_t version) noexcept;
>                               ^\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~
> In file included from
> tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:56:0:
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc: In function 'void*
> createInferRuntime_INTERNAL(void\*, int)':
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc:12:7: error:
> declaration of 'void* createInferRuntime_INTERNAL(void\*, int)' has a different
> exception specifier
>  void* createInferRuntime_INTERNAL(void* logger, int version) {
>        ^\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~
> In file included from
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:54:0,
>                  from tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:17:
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInferRuntime.h:2293:30:
> note: from previous declaration 'void* createInferRuntime_INTERNAL(void\*,
> int32_t) noexcept'
>  extern "C" TENSORRTAPI void* createInferRuntime_INTERNAL(void* logger,
> int32_t version) noexcept;
>                               ^\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~
> In file included from
> tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:56:0:
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc: In function
> 'nvinfer1::ILogger* getLogger()':
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc:19:20: error:
> declaration of 'nvinfer1::ILogger* getLogger()' has a different exception
> specifier
>  nvinfer1::ILogger* getLogger() {
>                     ^\~\~\~\~\~\~\~\~
> In file included from
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:54:0,
>                  from tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:17:
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInferRuntime.h:2309:43:
> note: from previous declaration 'nvinfer1::ILogger* getLogger() noexcept'
>  extern "C" TENSORRTAPI nvinfer1::ILogger* getLogger() noexcept;
>                                            ^\~\~\~\~\~\~\~\~
> In file included from
> tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:56:0:
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc: In function
> 'nvinfer1::IPluginRegistry* getPluginRegistry()':
> ./tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc:33:28: error:
> declaration of 'nvinfer1::IPluginRegistry* getPluginRegistry()' has a
> different exception specifier
>  nvinfer1::IPluginRegistry* getPluginRegistry() {
>                             ^\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~
> In file included from
> bazel-out/host/bin/external/local_config_tensorrt/\_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInfer.h:54:0,
>                  from tensorflow/compiler/tf2tensorrt/stub/nvinfer_stub.cc:17:
> bazel-out/host/bin/external/local_config_tensorrt/_virtual_includes/tensorrt_headers/third_party/tensorrt/NvInferRuntime.h:2304:51:
> note: from previous declaration 'nvinfer1::IPluginRegistry*
> getPluginRegistry() noexcept'
>  extern "C" TENSORRTAPI nvinfer1::IPluginRegistry* getPluginRegistry()
> noexcept;
>                                                    ^~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~
> Target //tensorflow/tools/pip_package:build_pip_package failed to build
> Use --verbose_failures to see the command lines of failed build steps.

将 tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_0.inc 第 19 行由
nvinfer1::ILogger* getLogger() {
改为
nvinfer1::ILogger* getLogger() noexcept {
第 33 行由
nvinfer1::IPluginRegistry* getPluginRegistry() {
改为
nvinfer1::IPluginRegistry* getPluginRegistry() noexcept {
tensorflow/compiler/tf2tensorrt/stub/NvInfer_5_1.inc 第 26 行由
nvinfer1::ILogger* getLogger() {
改为
nvinfer1::ILogger* getLogger() noexcept {
第 40 行由
nvinfer1::IPluginRegistry* getPluginRegistry() {
改为
nvinfer1::IPluginRegistry* getPluginRegistry() noexcept {

重新编译，报错，参考 [API Reference :: NVIDIA Deep Learning cuDNN Documentation](https://docs.nvidia.com/deeplearning/cudnn/api/index.html) 和 [cuDNN API :: NVIDIA Deep Learning SDK Documentation](https://docs.nvidia.com/deeplearning/cudnn/archives/cudnn_765/cudnn-api/index.html) 和 [cuDNN API :: NVIDIA Deep Learning SDK Documentation](https://docs.nvidia.com/deeplearning/cudnn/archives/cudnn_765/cudnn-api/index.html) 把

```c++
  RETURN_IF_CUDNN_ERROR(cudnnGetRNNDescriptor(
```

改为

```c++
  RETURN_IF_CUDNN_ERROR(cudnnGetRNNDescriptor_v6(
```

或将上下文改为

```c++
  cudnnDataType_t data_type, mathPrec;
  cudnnRNNMode_t cellMode;
  cudnnRNNBiasMode_t biasMode;
  cudnnMathType_t mathType;
  int32_t inputSize, projSize;
  uint32_t auxFlags
  RETURN_IF_CUDNN_ERROR(cudnnGetRNNDescriptor_v8(
      rnn_desc,//*handle=*/cudnn.handle(), /*rnnDesc=*/rnn_desc,
      &algo,//*hiddenSize=*/&hidden_size_v,
      &cellMode,//*numLayers=*/&num_layers_v,
      &biasMode,//*dropoutDesc=*/&dropout_desc,
      &direction,//*inputMode=*/&input_mode,
      &input_mode,//*direction=*/&direction,
      &data_type,//*mode=*/&mode,
      &mathPrec,//*algo=*/&algo,
      &mathType,//*dataType=*/&data_type));
      &inputSize,
      &hidden_size_v,
      &projSize,
      &num_layers_v,
      &dropout_desc,
      &auxFlags;
```

但是这无法解决

> tensorflow/stream_executor/cuda/cuda_dnn.cc:2319:3: error:
> 'cudnnConvolutionFwdPreference_t' was not declared in this scope
>    cudnnConvolutionFwdPreference_t preference =
>    ^\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~

的问题，因为该类型在 cuDNN 8 中已被完全移除，无法修复。如果不选择 CUDA 支持，那么接下来执行

```bash
sudo -E bazel build --config=v1 --config=cuda //tensorflow/tools/pip_package:build_pip_package
/bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
pip install ~/tensorflow_pkg/tensorflow-1.15.2-cp36-cp36m-linux_aarch64.whl
```

就可以成功构建出 whl。

解决方法参考 [NVIDIA Jetson TX2安装TensorFlow_yihuajack的博客-CSDN博客](https://blog.csdn.net/yihuajack/article/details/121234463) 用 NVIDIA 提供的 whl 安装，但这种方法只能安装 NVIDIA 提供的版本（目前是1.15.5、2.5.0 和 2.6.0）。
