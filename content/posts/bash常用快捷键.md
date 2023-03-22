+++ 
draft = false
date = 2023-03-17T15:48:55+08:00
title = "Bash常用快捷键"
description = "记录些bash快捷键"
slug = ""
authors = []
tags = ["杂记"]
categories = []
externalLink = ""
series = []
+++

[源贴](https://blog.csdn.net/force_eagle/article/details/7999153)

--------------

# Bash常用快捷键

## ctrl键组合

ctrl+a:光标移到行首。  
ctrl+b:光标左移一个字母  
ctrl+c:杀死当前进程。  
ctrl+d:退出当前 Shell。  
ctrl+e:光标移到行尾。  
ctrl+h:删除光标前一个字符，同 backspace 键相同。  
ctrl+k:清除光标后至行尾的内容。  
ctrl+l:清屏，相当于clear。  
ctrl+r:搜索之前打过的命令。会有一个提示，根据你输入的关键字进行搜索bash的history  
ctrl+u: 清除光标前至行首间的所有内容。  
ctrl+w: 移除光标前的一个单词  
ctrl+t: 交换光标位置前的两个字符  
ctrl+y: 粘贴或者恢复上次的删除  
ctrl+d: 删除光标所在字母;注意和backspace以及ctrl+h的区别，这2个是删除光标前的字符  
ctrl+f: 光标右移  
ctrl+z : 把当前进程转到后台运行，使用’ fg ‘命令恢复。比如top -d1 然后ctrl+z ，到后台，然后fg,重新恢复  

## esc组合

esc+d: 删除光标后的一个词  
esc+f: 往右跳一个词  
esc+b: 往左跳一个词  
esc+t: 交换光标位置前的两个单词。
