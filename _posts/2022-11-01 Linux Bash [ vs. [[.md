---
title: Linux Bash单方括号与双方括号 [和[[的区别
categories: Other Language
tags:
  - bash
  - linux
  - devops
  - 运维
date: 2022-11-01 01:52:58
---

本文参考 [Burak Gökmen](https://www.baeldung.com/linux/author/burakgokmen) 的文章 [Differences Between Single and Double Brackets in Bash](https://www.baeldung.com/linux/bash-single-vs-double-brackets)。
1. 单括号 [ 是 shell builtin，即 `test` 内置命令：
```text
$ type [
[ is a shell builtin
$ [ 3 -eq 3 ] && echo “Numbers are equal”
Numbers are equal
$ test 3 -eq 3 && echo “Numbers are equal”
Numbers are equal
```
[] 和 `test` 之间没有任何区别。[ 对于任何 Unix / Linux 系统的 Shell 都可用，因为它是 POSIX 兼容的。
双括号 [[ 是 shell keyword 关键字：
```text
$ type [[
[[ is a shell keyword
```
[[ 对于绝大多数 Shell 如 Bash、Zsh 等都可用，但是它并非 POSIX 兼容的。

2. 比较运算符（大于号 > 和小于号 <）在 [[]] 内可以直接使用，但在 [] 内需要转义：
```text
$ [[ 1 < 2 ]] && echo “1 is less than 2”
1 is less than 2
$ [ 1 < 2 ] && echo “1 is less than 2”
bash: 2: No such file or directory
$ [ 1 \< 2 ] && echo “1 is less than 2”
1 is less than 2
```
这是因为 [] 内将 < 和 > 默认当作是文件重定向符号。

3. 布尔运算符（且 && 和或 ||）在 [[]] 内可以直接使用，但在 [] 内只能用 `-a` 和 `-o`：
```text
$ [[ 3 -eq 3 && 4 -eq 4 ]] && echo “Numbers are equal”
Numbers are equal
$ [ 3 -eq 3 -a 4 -eq 4 ] && echo “Numbers are equal”
Numbers are equal
```
4. 表达式分组：在 [[]] 内可以直接用圆括号 () 分组，但在 [] 内需要转义：
```text
$ [[ 3 -eq 3 && (2 -eq 2 && 1 -eq 1) ]] && echo “Parentheses can be used”
Parentheses can be used
$ [ 3 -eq 3 -a (2 -eq 2 -a 1 -eq 1) ] && echo “Parentheses can be used”
bash: syntax error near unexpected token ‘(‘
$ [ 3 -eq 3 -a \( 2 -eq 2 -a 1 -eq 1 \) ] && echo “Parentheses can be used”
Parentheses can be used
```
并且 [] 内的圆括号前后都需要加空格。

5. 模式匹配仅在 [[]] 内有效，在 [] 内无效：
```text
$ name=”Alice”
$ [[ $name = *c* ]] && echo “Name includes c”
Name includes c
$ echo $?
0
$ [ $name = *c* ] && echo “Name includes c”
$ echo $?
1
```
6. 正则表达式仅在 [[]] 内有效，在 [] 内无效：
```text
$ name=”Alice”
$ [[ $name =~ ^Ali ]] && echo ”Regular expressions can be used”
Regular expressions can be used
$ [ $name =~ ^Ali ] && echo ”Regular expressions can be used”
bash: [: =~: binary operator expected
```
其中，`=~` 内置运算符表示正则表达式匹配。

7. 词分割：如果一个变量是一个包含空格的字符串，那么在 [[]] 内字符串不会被分割成多个词，但是在 [] 内会被分割成多个词：
```text
$ filename=”nonexistent file”
$ [[ ! -e $filename ]] && echo ”File doesn’t exist”
File doesn’t exist
$ [ ! -e $filename ] && echo ”File doesn’t exist”
bash: [: nonexistent: binary operator expected
```
如果想在 [] 内避免这种情况，需要将变量用双引号包裹：
```text
$ filename=”nonexistent file”
$ [ ! -e “$filename” ] && echo ”File doesn’t exist”
File doesn’t exist
```
**总结而言，[[ 是增强版的 [。如果追求绝对的兼容性，那么应使用 [，但是一般而言追求方便，使用 [[ 即可。**