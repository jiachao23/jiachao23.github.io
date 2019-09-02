---
layout: post
title: 谷歌Auto的使用
categories: [translate]
description: 谷歌Auto的使用
keywords: translate, auto
---

#### 作者网页：[www.jcohy.com](http://www.jcohy.com)  	
> #### PS:待开发中。。。。

#### 项目简介

Java源代码生成器集合。

Java充满了机械的，重复的，未经测试的代码。有时会发生一些微小的错误。就像一个机器人在做一些任务。

[项目地址](https://github.com/google/auto)

#### 子项目

- AutoFactory：兼容JSR-330的工程

> 
- AutoService：为ServiceLoader提供程序配置文件

> 生成具有ServiceLoader配置/元数据样式的生成器。Java注解处理器和其他系统使用java.util.ServiceLoader来提供的META-INF元数据注册已知类型的实现。但是，开发人员很容易忘记更新或正确指定服务描述符。
AutoService为开发人员生成此元数据，用于使用@AutoService注解的任何类，避免拼写错误，提供对重构错误的抵制等。

- AutoValue：Java 1.6+的不可变值类型代码生成。

- Common：用于编写注解处理器的有用的处理工具。

#### 使用



