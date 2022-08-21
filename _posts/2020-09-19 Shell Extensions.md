---
title: Shell插件合集
categories: CSDN补档
tags:
  - shell
  - lsd
  - croc
  - zoxide
  - fzf
abbrlink: 21934
date: 2020-09-19 02:44:44
---

1. “The next gen ls command” [lsd](https://github.com/Peltoche/lsd)：

   Ubuntu: 先在[Release页面](https://github.com/Peltoche/lsd/releases)下载deb包，然后使用dpkg命令安装：

   ```
   wget https://github.com/Peltoche/lsd/releases/download/0.18.0/lsd_0.18.0_amd64.deb
   
   
   
   sudo dpkg -i lsd_0.18.0_amd64.deb
   ```

   Windows：

   ```bash
   scoop install lsd
   ```

    

2. “Easily and securely send things from one computer to another” [croc](https://github.com/schollz/croc)：

   Ubuntu：

   ```bash
   snap install croc
   ```

   （WSL无法使用snap，使用

   ```bash
   curl https://getcroc.schollz.com | bash
   ```

   代替）
   Windows：

   ```bash
   scoop install croc
   ```

    

3. “A faster way to navigate your filesystem”  [zoxide](https://github.com/ajeetdsouza/zoxide)：

   Ubuntu：

   ```bash
   cargo install zoxide -f
   ```

   Windows：

   ```bash
   scoop install zoxide
   ```

   在~/.bashrc中添加

   ```bash
   eval "$(zoxide init bash)"
   ```

   在~/.zshrc中添加

   ```bash
   eval "$(zoxide init zsh)"
   ```

   在C:\Users\<username>\Documents\PowerShell\Microsoft.PowerShell_profile.ps1中添加

   ```bash
   Invoke-Expression (& {
       $hook = if ($PSVersionTable.PSVersion.Major -lt 6) { 'prompt' } else { 'pwd' }
       (zoxide init --hook $hook powershell) -join "`n"
   })
   ```

    

4. “A command-line fuzzy finder”  [fzf](https://github.com/junegunn/fzf)：

   Ubuntu：

   ```bash
   sudo apt install fzf
   ```

   Windows：

   ```bash
   scoop install fzf
   ```

   或

   ```bash
   choco install fzf
   ```

    

5. “Vim-fork focused on extensibility and usability” [neovim](https://github.com/neovim/neovim)：

   Ubuntu：

   ```bash
   sudo apt install neovim
   ```

   Windows：

   ```bash
   scoop install neovim
   ```

   或

   ```bash
   choco install neovim
   ```