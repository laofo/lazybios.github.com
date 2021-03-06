---
layout: post
title: /boot文件清理及vim&tmux
categories: 
- Linux
tags:
- vim
- Linux
- tmux
---

单纯通过阅读识记的方法来学习linux是不会与多大进展的，因为linux中有大量的命令行操作，要做到熟练操作，仅仅是通过脑袋中的背诵记忆的方式是不会有所突破的，其实命令行的操作就像是学习拧转魔方，如果你是靠推理的方式来完成6个面的话，估计要花你大把的时间，据说一个英国小伙花了26年，但实际上现在的国际记录是2011年单次5.66秒平均7.64秒，看到这个数字，估计大多数人都会吃惊，其实仔细想想也没什么可惊的，魔方的高级玩法有100多个公式，熟练记住和运用这些公式，大概能让你提速到30秒内，我想100多个公式记忆绝对不需要26年的，如果说惊叹那也只能佩服那人学习记忆练习的毅力非凡，同样的linux的熟练操作也是“无它，唯手熟尔！”就是练习肌肉记忆，当你慢下来时估计你自己都搞不清怎么做的，这个时候就叫肌肉记忆，据说中国乒乓球国家对的训练项里就有这样的一项练习挥拍角度，为的就是练习对一些场景的下意识 其练习效果就每次看奥运你也懂得。有点扯远了，总之我个人感觉linux学习不动手是不行的，以后我会将每次使用linux过程中的新知识和关键点做笔记贴出来，一次为了让自己备查，另外希望也能帮到那些遇到同样问题的同学，当然也不可能一次性将所有的东西写完美，每次只做日志记录，多次迭代完善记录，最后阶段性的去归类总结之前的操作日志为特定主题的文章

####/boot 文件夹空间不足
装系统的时候给/boot 文件夹分了100MB的容量，但因为系统升级的缘故，每次升级Ubuntu不仅会更新最新到最新内核，同时也会保存旧有的内核文件在/boot下，其实100MB已是足够的，只要删除非当前不用，只留2个早版本的内核文件以备当前内核崩溃时候可以替换上去就可以了，具体用到的命令有：

> `df -lh` 查看硬盘的挂载情况

> `uname -a` 查看当前内核版本

> `dpkg --get-selections | grep linux-image ` 产看内核文件有哪些

> `sudo apt-get remove linux-image-xxx.xxx ` 根据内核列表删除无用的内核文件

再引用鸟哥书中的话：
>/boot 这个目录主要在放置开机会使用到的文件，包括Linux核心文件以及开机选单与开机所需配置文件等等。 Linux kernel常用的档名为：vmlinuz，如果使用的是grub这个开机管理程序， 则还会存在/boot/grub/这个目录喔！

####linux外接显示器
+ 安装显卡驱动，这里的驱动如果给出了好几项，最好用最新的，不要用推荐的老版本驱动，网上说N卡好找一些，A卡总遇到问题，不知道是不是这么一回事儿，我的是N卡
+ `sudo apt-get install nvidia-settings` 安装nvidia设置
+ win建 搜nvidia 出来的设置文件中 看是否驱动成功能否找到另外一个显示器信号源，一般情况这里就算成功了，最后到系统设置-显示 中开启另一个显示器就可以了

忘了交代了我用的是Ubuntu 12.04 LTS版，最后上张效果图：
![Imgur](http://i.imgur.com/uNKc4vP.jpg)

####tmux
这是款复用终端的软件，可以将一个终端根据水平、垂直等方向切割成多个终端会话，从而节省了屏幕资源，同时又可以进行完全不同的操作,上图右侧显示器显示画面就为tmux实际使用效果图

+ 安装 `sudo apt-get install tmux` 进入直接在控制台下键入`tmux`就行
+ 基本常用操作 `Ctrl+b`激活控制台之后`q`显示面板编号,`o`移动到当前下一个面板,`%`垂直切分当前窗口，`\"`水平切分当前窗口

基本常用指令就是这些，更多高级功能见[这里](http://baike.baidu.com/view/9065064.htm)

####Vim设置
+ 默认情况vim的tab是8个空格，这样的话其实并不美观，反而有点浪费屏幕空间，通过需igai`vim ~/.vimrc`配置文件，在其中添加 `set tabstop=4`即可实现tab变4个空格
+ 光标移动选择 在可是情况下按`v`之后通过`hjkl`移动光标进行选择，`y`复制，`d`剪切，`p`粘贴

> 复制\剪切一行则：`yy` \ `dd`

> 复制\剪切当前光标所在的位置到行尾：`y$` \ `d$`

> 复制\剪切当前光标所在的位置到行首：`y^` \ `d^`

> 复制\剪切三行则：`3yy` \ `3dd`，即从当前光标+下两行。



