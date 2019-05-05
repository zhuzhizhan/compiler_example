# Compiler Example1

发布版本：1.0

作者邮箱：703632667@qq.com

日期：2019.05

文件密级：公开资料

------

**前言**

**概述**

gcc编译器简单使用例程.

**读者对象**

本文档（本指南）主要适用于以下工程师：

技术支持工程师

软件开发工程师

**产品版本**

**修订记录**

| **日期**   | **版本** | **作者**  | **修改说明** |
| ---------- | -------- | --------- | ------------ |
| 2019-05-05 | V1.0     | Jason Zhu | 初始版本     |

------

[TOC]

------

## 1 介绍

### 1.1 预处理

arm-linux-gnueabi-gcc -E test.c -o test.i

### 1.2 编译

arm-linux-gnueabi-gcc -S test.i -o test.s

### 1.3 汇编

arm-linux-gnueabi-gcc -c test.s -o test.o

### 1.4 链接

arm-linux-gnueabi-gcc test.o -o test --static
