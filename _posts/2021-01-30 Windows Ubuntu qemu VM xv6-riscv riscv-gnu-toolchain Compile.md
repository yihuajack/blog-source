---
title: Windows/Ubuntu qemu虚拟机xv6-riscv利用riscv-gnu-toolchain编译安装方法
categories: SuperUser
tags:
  - risc-v
  - xv6
  - qemu
  - makefile
  - os
date: 2021-01-30 00:21:45
---

本文参考2019年版的[Tools Used in 6.828](https://pdos.csail.mit.edu/6.828/2019/tools.html)（注意到当前版本也就是2020版本的[Xv6, a simple Unix-like teaching operating system](https://pdos.csail.mit.edu/6.828/2020/xv6.html)并没有清楚的instruction）。以下两种方法无论是使用apt安装qemu-system-misc还是编译安装qemu皆可，但不要使用apt安装qemu。

方法一：适用于bulleyes/sid版本的Debian系统（可通过执行

```bash
cat /etc/debian_version
```

查看Debian版本）

执行

```bash
sudo apt install git build-essential gdb-multiarch qemu-system-misc gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu
```

git、build-essential一般已安装过；如之前已编译安装过qemu无需再安装qemu-system-misc，然后跳转至下文【*】处说明继续。

方法二：Linux通用（自己编译）

1. 根据riscv-gnu-toolchain在Ubuntu虚拟机中执行
   ```bash
   sudo apt-get install autoconf automake autotools-dev curl python3 libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
   ```

   由于riscv-gnu-toolchain源码已有逾6.65GiB之多，建议先检查虚拟机分配的空间是否充足，一般默认分配的20GiB空间几乎不可能够用，因此需要先扩展虚拟机磁盘，可以参考[VMware虚拟机扩展Ubuntu系统磁盘空间](https://www.cnblogs.com/dongry/p/10620894.html)、[完美解决VMware虚拟机 Linux系统 Ubuntu 20.04 硬盘/磁盘扩容的问题（超级超级详细）](https://blog.csdn.net/weixin_41194129/article/details/107429956)等，但由于系统差异部分地方有所出入，建议直接扩展到40GiB，无需手动配置/dev/sda2和/dev/sda5，设置时会自动限制为swapfile留出至少约1GiB空间。

2. 如前所述该库全部克隆大小很大，而且分散在各个submodule中，其中子模块riscv-newlib源在sourceware.org上，子模块qemu源在git.qemu.org上，速度比GitHub更慢。尽管README中已说明直接克隆该库本身，在make过程中会自动克隆各子模块，但对于大多数网络环境几乎不可能一次性克隆成功，即使使用--recursive参数也容易失败。Gitee上的库的子模块riscv-binutils-gdb、riscv-dejagnu、riscv-gcc、riscv-binutils-gdb、riscv-glibc会自动从Gitee上的镜像克隆，而GitHub上的库的这些子模块都从GitHub上克隆，即使使用代理也很难克隆成功，因此直接从Gitee上克隆库：
   ```bash
   git clone git@gitee.com:mirrors/riscv-gnu-toolchain.git
   ```

   （注意克隆会在Windows系统上报错，这是由于Windows系统是大小写不敏感的，而Linux等系统是大小写敏感的，这会导致linux-headers/include文件夹下的几个文件发生名字冲突而只保留一个。）这时如果直接克隆子模块会很容易失败，这是由于默认的.gitmodules中使用的qemu和riscv-newlib仍是其他git上的源，所以需要手动更改如下：
   [submodule "riscv-newlib"]
       path = riscv-newlib
       url = git@github.com:mirror/newlib-cygwin.git
       branch = master
   [submodule "qemu"]
       path = qemu
       url = git@gitee.com:mirrors/qemu.git
   注意这里riscv-newlib采用的是GitHub上的newlib-cygwin库，这是默认的git://sourceware.org/git/newlib-cygwin.git库的镜像，而另一个库riscv-newlib在GitHub和Gitee上都有，其内容却与newlib-cygwin有所不同，为保证一致采用了newlib-cygwin。

3. 执行
   ```bash
   cd riscv-gnu-toolchain/qemu
   git submodule update --init --recursive
   ```

   克隆子模块，克隆期间Shell并不会显示时间和完成比例，可以执行
   ```bash
   sudo apt install nethogs
   sudo nethogs
   ```

   查看网络流量信息，通过查看ssh的下载速度判断是否正在进行克隆。

4. 在riscv-gnu-toolchain目录下执行
   ```bash
   ./configure --prefix=/opt/riscv
   sudo make
   ```

   编译安装Newlib交叉编译器，取决于虚拟机的速度可能要执行约两个小时。

5. 编译安装QEMU（使用apt安装qemu-system-misc也可）：根据[Download QEMU](https://www.qemu.org/download/#source)在~目录下执行
   ```bash
   git clone https://gitlab.com/qemu-project/qemu.git
   cd qemu
   git submodule init
   git submodule update --recursive
   ./configure
   sudo make
   sudo make install
   ```

   编译安装qemu，在configure阶段可能会报错缺少Ninja和pixman-1，执行

   ```bash
   sudo apt install ninja-build
   sudo apt install libpixman-1-dev
   ```
   安装依赖项，make过程根据机器速度需要花费数小时的时间。安装结束后将安装目录/usr/local/bin添加到环境变量

6. ```bash
   export PATH=/usr/local/bin:$PATH
   ```

   执行该命令临时生效或将其添加到~/.bashrc或~/.zshrc以对当前用户永久生效。

7. 执行
   ```bash
   riscv64-unknown-elf-gcc --version
   ```
   
   检查riscv-gnu-toolchain是否已成功安装。

【*】执行

```bash
qemu-system-riscv64 --version
```

检查qemu是否已成功安装。在~目录下执行

```bash
git clone git@github.com:mit-pdos/xv6-riscv.git
cd xv6-riscv
make qemu
```

完成安装。注意这里make qemu不可以在root中执行，也不可以加sudo，否则会报错：

\***
\*** Error: Couldn't find a riscv64 version of GCC/binutils.
\*** To turn off this error, run 'gmake TOOLPREFIX= ...'.
\***
检查源代码的Makefile文件找到38-49行：

```makefile
ifndef TOOLPREFIX
TOOLPREFIX := $(shell if riscv64-unknown-elf-objdump -i 2>&1 | grep 'elf64-big' >/dev/null 2>&1; \
	then echo 'riscv64-unknown-elf-'; \
	elif riscv64-linux-gnu-objdump -i 2>&1 | grep 'elf64-big' >/dev/null 2>&1; \
	then echo 'riscv64-linux-gnu-'; \
	elif riscv64-unknown-linux-gnu-objdump -i 2>&1 | grep 'elf64-big' >/dev/null 2>&1; \
	then echo 'riscv64-unknown-linux-gnu-'; \
	else echo "***" 1>&2; \
	echo "*** Error: Couldn't find a riscv64 version of GCC/binutils." 1>&2; \
	echo "*** To turn off this error, run 'gmake TOOLPREFIX= ...'." 1>&2; \
	echo "***" 1>&2; exit 1; fi)
endif
```

在Terminal中执行

```bash
riscv64-unknown-elf-objdump -i
```

检查输出中的确有'elf64-big'，执行

```bash
riscv64-unknown-elf-objdump -i 2>&1 | grep 'elf64-big' 2>&1iscv64-unknown-elf-objdump -i
```

（去掉>/dev/null以获得输出）显示
elf64-big
         elf64-littleriscv elf32-littleriscv elf64-little elf64-big 
   riscv elf64-littleriscv elf32-littleriscv elf64-little elf64-big
说明条件语句条件为真却没有通过，这是因为执行

```bash
sudo riscv64-unknown-elf-objdump -i
```

会报错sudo: riscv64-unknown-elf-objdump: command not found。
make时可能出现make: *** No rule to make target '/usr/lib/gcc-cross/riscv64-linux-gnu/9/include/stdarg.h', needed by 'kernel/console.o'.  Stop.错误，受Why does gcc search header files from non-exist folders?启发，执行

```bash
make clean
```

清理之前make的文件即可。

【注意】make qemu过程会卡在qemu-system-riscv64 -machine virt -bios none -kernel kernel/kernel -m 128M -smp 3 -nographic -drive file=fs.img,if=none,format=raw,id=x0 -device virtio-blk-device,drive=x0,bus=virtio-mmio-bus.0处，这种情况已经在Tools Used in 6.S081中指出【2021年2月18日更新】，需执行

```bash
sudo apt-get remove qemu-system-misc
sudo apt-get install qemu-system-misc=1:4.2-3ubuntu6
```

或者在Windows系统上也克隆一份xv6-riscv并安装qemu，通过研究xv6-riscv的Makefile，将虚拟机中的xv6-riscv文件夹下的kernel文件夹下的kernel文件复制到Windows中的xv6-riscv文件夹的kernel文件夹中，将xv6-riscv中已经make出来的fs.img镜像文件复制到Windows中的xv6-riscv文件夹下，然后在CMD或PowerShell中执行

```powershell
qemu-system-riscv64 -machine virt -bios none -kernel kernel/kernel -m 128M -smp 3 -nographic -drive file=fs.img,if=none,format=raw,id=x0 -device virtio-blk-device,drive=x0,bus=virtio-mmio-bus.0
```

显示
xv6 kernel is booting

hart 1 starting
hart 2 starting
$
即成功启动xv6。