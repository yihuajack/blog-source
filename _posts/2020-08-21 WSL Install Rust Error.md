---
title: WSL1安装rust报错thread ‘main‘ panicked的解决方法
categories: CSDN补档
tags:
  - rust
  - vundle
  - vim
  - wsl
abbrlink: 10972
date: 2020-08-21 13:33:07
---

根据https://www.rust-lang.org/tools/install，WSL安装rust使用命令

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

报错

```
info: downloading installer
 
Welcome to Rust!
 
This will download and install the official compiler for the Rust
programming language, and its package manager, Cargo.
 
Rustup metadata and toolchains will be installed into the Rustup
home directory, located at:
 
  /home/ayka_tsuzuki/.rustup
 
This can be modified with the RUSTUP_HOME environment variable.
 
The Cargo home directory located at:
 
  /home/ayka_tsuzuki/.cargo
 
This can be modified with the CARGO_HOME environment variable.
 
The cargo, rustc, rustup and other commands will be added to
Cargo's bin directory, located at:
 
  /home/ayka_tsuzuki/.cargo/bin
 
This path will then be added to your PATH environment variable by
modifying the profile files located at:
 
  /home/ayka_tsuzuki/.profile
  /home/ayka_tsuzuki/.zprofile
 
You can uninstall at any time with rustup self uninstall and
these changes will be reverted.
 
Current installation options:
 
 
   default host triple: x86_64-unknown-linux-gnu
     default toolchain: stable (default)
               profile: default
  modify PATH variable: yes
 
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>1
 
info: profile set to 'default'
info: default host triple is x86_64-unknown-linux-gnu
info: syncing channel updates for 'stable-x86_64-unknown-linux-gnu'
462.8 KiB / 462.8 KiB (100 %) 108.0 KiB/s in  4s ETA:  0s
info: latest update on 2020-08-03, rust version 1.45.2 (d3fb005a3 2020-07-31)
info: downloading component 'cargo'
info: downloading component 'clippy'
info: downloading component 'rust-docs'
info: downloading component 'rust-std'
info: downloading component 'rustc'
info: downloading component 'rustfmt'
info: installing component 'cargo'
info: Defaulting to 500.0 MiB unpack ram
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `22`,
 right: `4`', src/libstd/sys/unix/thread.rs:179:21
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `22`,
 right: `4`', src/libstd/sys/unix/thread.rs:179:21
stack backtrace:
   0:     0x7f0f95edaffd - backtrace::backtrace::libunwind::trace::h812748238d609e46
                               at /cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.46/src/backtrace/libunwind.rs:86
   1:     0x7f0f95edaffd - backtrace::backtrace::trace_unsynchronized::h7c97e818aebf09c8
                               at /cargo/registry/src/github.com-1ecc6299db9ec823/backtrace-0.3.46/src/backtrace/mod.rs:66
   2:     0x7f0f95edaffd - std::sys_common::backtrace::_print_fmt::h60d914263b0ccd71
                               at src/libstd/sys_common/backtrace.rs:78
   3:     0x7f0f95edaffd - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::hf78227137afc7565
                               at src/libstd/sys_common/backtrace.rs:59
   4:     0x7f0f95b6524c - core::fmt::write::h543cdf60775f89bf
                               at src/libcore/fmt/mod.rs:1069
   5:     0x7f0f95eda904 - std::io::Write::write_fmt::h0c7f3ce24c679426
                               at src/libstd/io/mod.rs:1504
   6:     0x7f0f95eda105 - std::sys_common::backtrace::_print::h80e55e24be231368
                               at src/libstd/sys_common/backtrace.rs:62
   7:     0x7f0f95eda105 - std::sys_common::backtrace::print::h3b197b9c1261c865
                               at src/libstd/sys_common/backtrace.rs:49
   8:     0x7f0f95eda105 - std::panicking::default_hook::{{closure}}::ha6c807149ce20f8f
                               at src/libstd/panicking.rs:198
   9:     0x7f0f95ed98f4 - std::panicking::default_hook::he49a9c12e358cc45
                               at src/libstd/panicking.rs:218
  10:     0x7f0f95ed93b6 - std::panicking::rust_panic_with_hook::h93f74f5ef2f71f31
                               at src/libstd/panicking.rs:515
  11:     0x7f0f95ed9198 - rust_begin_unwind
                               at src/libstd/panicking.rs:419
  12:     0x7f0f95ed9140 - std::panicking::begin_panic_fmt::hfa6ef29ba81f400e
                               at src/libstd/panicking.rs:373
  13:     0x7f0f95e11671 - <rustup::diskio::threaded::Threaded as rustup::diskio::Executor>::join::habf6ee901a47a8a4
  14:     0x7f0f95e10c78 - core::ptr::drop_in_place::h0b1b0dcb4b07c267
  15:     0x7f0f95d76c40 - core::ptr::drop_in_place::h09572fbec04e583f
  16:     0x7f0f95e2842e - rustup::dist::component::package::unpack_without_first_dir::h99f4b0c2cae349ff
  17:     0x7f0f95de559f - rustup::dist::manifestation::Manifestation::update::hacf67fad3b44f03a
  18:     0x7f0f95dd563c - rustup::dist::dist::update_from_dist_::h1462627f9c58c494
  19:     0x7f0f95dd227a - rustup::install::InstallMethod::install::hbb6430b2dfaf5792
  20:     0x7f0f95dd0d21 - rustup::toolchain::DistributableToolchain::install_from_dist::h7b9bf9317c9aa2c5
  21:     0x7f0f95ec5aa2 - rustup::cli::self_update::install::h8ca0f61a773526a9
  22:     0x7f0f95ecbd3d - rustup::cli::setup_mode::main::h4d335e1871435d70
  23:     0x7f0f95adf42a - rustup_init::main::h22da971c634937a2
  24:     0x7f0f95ef3e83 - std::rt::lang_start_internal::{{closure}}::{{closure}}::h4ed4ab1fb893cc93
                               at src/libstd/rt.rs:52
  25:     0x7f0f95ef3e83 - std::sys_common::backtrace::__rust_begin_short_backtrace::h1f01c818c00c4f70
                               at src/libstd/sys_common/backtrace.rs:130
  26:     0x7f0f95ae053f - main
  27:     0x7f0f956a70b3 - __libc_start_main
  28:     0x7f0f95adb029 - <unknown>
thread panicked while panicking. aborting.
Illegal instruction (core dumped)
```

参考GitHub库rust-lang/rustup的issue #2245：[rustup panic with WSLv1 + glibc 2.31](https://github.com/rust-lang/rustup/issues/2245)，一种办法是将WSL1升级到WSL2：

```bash
wsl --set-default-version 2
wsl --list --verbose
wsl --set-version Ubuntu-20.04 2
```

然后再执行安装命令；另一种办法是在.bashrc或.bash_profile（如果用的Shell是ZSH则在.zshrc）中插入

```
export RUSTUP_IO_THREADS=1
```

然后source .zshrc，或直接在命令行中输入该命令执行，然后再执行安装rust命令，即可成功安装：

```
info: downloading installer
 
Welcome to Rust!
 
This will download and install the official compiler for the Rust
programming language, and its package manager, Cargo.
 
Rustup metadata and toolchains will be installed into the Rustup
home directory, located at:
 
  /home/ayka_tsuzuki/.rustup
 
This can be modified with the RUSTUP_HOME environment variable.
 
The Cargo home directory located at:
 
  /home/ayka_tsuzuki/.cargo
 
This can be modified with the CARGO_HOME environment variable.
 
The cargo, rustc, rustup and other commands will be added to
Cargo's bin directory, located at:
 
  /home/ayka_tsuzuki/.cargo/bin
 
This path will then be added to your PATH environment variable by
modifying the profile files located at:
 
  /home/ayka_tsuzuki/.profile
  /home/ayka_tsuzuki/.zprofile
 
You can uninstall at any time with rustup self uninstall and
these changes will be reverted.
 
Current installation options:
 
 
   default host triple: x86_64-unknown-linux-gnu
     default toolchain: stable (default)
               profile: default
  modify PATH variable: yes
 
1) Proceed with installation (default)
2) Customize installation
3) Cancel installation
>1
 
info: profile set to 'default'
info: default host triple is x86_64-unknown-linux-gnu
info: syncing channel updates for 'stable-x86_64-unknown-linux-gnu'
462.8 KiB / 462.8 KiB (100 %) 416.0 KiB/s in  1s ETA:  0s
info: latest update on 2020-08-03, rust version 1.45.2 (d3fb005a3 2020-07-31)
info: downloading component 'cargo'
info: downloading component 'clippy'
info: downloading component 'rust-docs'
info: downloading component 'rust-std'
info: downloading component 'rustc'
info: downloading component 'rustfmt'
info: installing component 'cargo'
info: Defaulting to 500.0 MiB unpack ram
info: installing component 'clippy'
info: installing component 'rust-docs'
 12.2 MiB /  12.2 MiB (100 %) 459.2 KiB/s in 16s ETA:  0s
info: installing component 'rust-std'
 15.9 MiB /  15.9 MiB (100 %)  11.8 MiB/s in  1s ETA:  0s
info: installing component 'rustc'
 47.1 MiB /  47.1 MiB (100 %)  11.6 MiB/s in  4s ETA:  0s
info: installing component 'rustfmt'
info: default toolchain set to 'stable'
 
  stable installed - rustc 1.45.2 (d3fb005a3 2020-07-31)
```

【注意：使用Vundle安装vim插件YCM（YouCompleteMe）时需要使用Rust编译安装会自动安装Rust也会出现该问题，也需使用该方法解决】