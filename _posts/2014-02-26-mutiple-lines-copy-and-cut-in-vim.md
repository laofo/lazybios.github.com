---
layout: post
title: vim多行复制/粘贴
categories:
- VIM
tags:
- vim
- copy&cut
---

#####不计行数选区复制
移动光标到待复制区域的起始位置，`mk`标记该位置为`k`   
再移动光标到待复制区域的末尾位置，`y'k` 即为从开始标记复制到当前位置    

然后在normal模式下就可以使用`p`来赋值到任何位置

#####通过行数区域复制
normal模式下，通过计数行数，与命令相结合     
`:100y` #复制第100行    
`:100,150y` #复制第100行到150行

用样在normal模式下就可以使用`p`来赋值到任何位置

同样道理也可以使用'd'来剪切或删除特定到行或多行,操作如上！






