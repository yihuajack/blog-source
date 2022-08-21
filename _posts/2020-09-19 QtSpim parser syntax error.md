---
title: QtSpim写MIPS报错spim parser syntax error等各种错误的解决方法
categories: CSDN补档
tags:
  - assembler
  - mips
  - spim
  - qtspim
abbrlink: 12191
date: 2020-09-19 17:03:53
---

1. spim: (parser) syntax error on line 2 of file xxx

   .word 10,11,12,13,14,15,16,17,18

      ^

   line 2:

   ```
   A:  .word 10,11,12,13,14,15,16,17,18
   ```

   改为

   ```
   A:  .word 10, 11, 12, 13, 14, 15, 16, 17, 18
   ```

   或

   ```
   A:  .word 10:18
   ```

2. spim: (parser) syntax error on line 5 of file xxx

   j: .word 0

    ^

   line 5:

   ```
   j:  .word 0
   ```

   改为

   ```
   J:  .word 0
   ```

3. spim: (parser) Label is defined for the second time on line 8 of file xxx
   main:
     ^
   line 8:

   ```
   main:
   ```

   根据CS 265 George Mason University D. Nordstrom的[A Quick Introduction to SPIM](https://cs.gmu.edu/~dnord/cs265/spim_intro.html)，在QtSpim的菜单栏File->Reinitialize and Load File后即可解决该问题。

4. spim: (parser) syntax error on line 14 of file xxx
   lw $t2,$t1($s5)
      ^
   line 14:

   ```
   lw $t2,$t1($s5)
   ```

   改为

   ```
   add $t1,$s5,$t1
   lw $t2,0($t1)
   ```

5. Attempt to execute non-instruction at 0x00400030
   根据[Attempt to execute non-instruction in mips assembler?](https://stackoverflow.com/questions/20172655/attempt-to-execute-non-instruction-in-mips-assembler)在.s文件代码最后一行添加

   ```
   jr $ra
   ```