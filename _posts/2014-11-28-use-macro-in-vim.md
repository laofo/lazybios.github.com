---
layout: post
title: vim中宏录制与调用
categories:
- VIM
tags:
- linux
- vim
---

在一般的场景中也用不到宏，但是今天在生成json格式的数据集的过程中，大几千条数据，结果发现忘输`,`逗号了，想想肯定不可能手动去添加逗号的，难道要
重新生成一遍吗？虽然也费不了多少时间，但想想是否还有更好地方法，在vim中高效的解决，于是想到了这个使用vim以来从未接触过的功能，**录制宏**，大概
搜了一下，发现正适合当前的使用场景具体录制过程

+ normal模式下按q+任意a~z中一个字母，例如qc,即表示开始录制，且这个宏的名字叫做c，之后会使用这个c的名字调用这个宏
+ 之后对json数据任意一行如下操作 `$a,[Esc]j` 即表示将光标移动到行尾并追加一个`,`在行末尾，Esc从当前行跳到下一行
+ 在此输入`q`，即表示退出宏c的录制
+ 调用即在待处理行执行n@c即可以播放刚才录制的宏命令，实现自动追加n次，前面的n表示播放宏c的次数

再来举个更实用的例子,行序号自增，假设行内容为`0 item`



+ normal 光标移动到`0 item`行上  qa 进入录制宏a的模式
+ 输入`Ypctrl+a` 这行的意思是首先复制当前行，之后粘贴改行，然后对行首的数字进行自增1运算,需要说明的是ctrl+a与ctrl+x分别表示对数字`+1`和`-1`,可以自动识别8进制和16进制
+ 输入q退出，宏a的录制
+ 调用100@a，播放100次刚才的录制，即产生100数据，起行首的数字最后一行为101

That's all!


### 参考内容
[http://liuzhijun.iteye.com/blog/1844845](http://liuzhijun.iteye.com/blog/1844845)
