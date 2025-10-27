---
title: linux中gdb调试工具的使用
published: 2025-10-26
description: 'gdb工具介绍及基础使用'
image: ''
tags: [gdb调试]
category: '教程'
draft: false 
lang: 'cn'
---

# GDB

## 要进行gdb调试必须了解

程序存在两种开发版本（debug/release），debug版本是给程序员用来调试的，而release版本是让用户使用的，两者的区别是debug开发版本中包含了程序的调试信息，内存占用会比release模式大

因此，要进行gdb调试，必须是用debug开发版本进行调试

> g++编译器默认是编译出release版本，debug需要在g++后添加-g选项
> 
> g++ -g myproject.cpp -o myproject

## GDB指令学习

* 指令集汇总

> `l(list)`    :    行号/函数名------>显示对应代码，默认10行
> 
> `r(run)`    :    运行程序
> 
> `b(break/breakpoint) + 行号/函数名`     :    在指定行号或者函数名处打上断点
> 
> `info b`    :  查看断点信息
> 
> `d(delete) + 断点编号`    :    删除断点
> 
> `d + breakpoints`    :    删除所有断点
> 
> `n(next)`    :    逐过程进行程序（判断哪个函数体出错）
> 
> `s(step)`    :    逐语句进行程序（一步步调试程序）
> 
> `bt`    :    查看函数栈帧（即当前函数的调用关系）
> 
> `set + 变量名`    :    设置变量的值
> 
> `p(print) + 变量名`    :    打印变量值（还可以print *this 打印类的属性值）
> 
> `display + 变量名`    :    跟踪查看一个变量，每次执行gdb都显示值
> 
> `undisply + 变量名编号`    :    取消对先前设置的变量的跟踪
> 
> ---------------------------常用排查问题的三剑客
> 
> `until + 行号`    :    进行指定位置的跳转，执行区间所有代码
> 
> `finish`    :    直接执行完当前函数
> 
> `c(continue)`    :    从一个断点处，直接运行到下一个断点处

## 参考文献

[【Linux】GDB保姆级调试指南（什么是GDB？GDB如何使用？）_gdb教程-CSDN博客](https://blog.csdn.net/weixin_45031801/article/details/134399664?spm=1001.2014.3001.5506)
