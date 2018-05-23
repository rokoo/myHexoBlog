---
title: shim层概念理解
catalog: true
date: 2018-05-22 19:39:05
subtitle:
header-img: "/img/header_img/header-bg-railway.jpg"
tags:
- shim
- concept
---
> 最近在做区块链项目，在学习[Hyperledger](https://www.hyperledger.org/) 时需要看很多英文资料，很多次看到shim这个名词，不是很理解，做了下作业，发现有个解释比较好理解，在这里翻译一下供大家参考。

# shim概念理解
以下是原文
> A shim is typically something written specifically to maintain backwards compatibility. For example, if you have two versions of an API, version 1 and version 2, then rather than maintaining version 1 independently of version 2, you might write a shim that intercepts calls to version 1 of the API, translates the parameters to what version 2 requires, and then returns the results.
>Typically, a shim is written by the provider of the API, rather than consumer.
>A wrapper is written by the consumer of an API and is typically written so that you can switch the underlying API without the rest of your code having to know. For example, you might write a database wrapper so that you can talk to MS SQL Server, or Oracle, just by switching wrappers.
>Of course, as with any terminology, there are gray areas, but I think the above covers the main differences.

意思就是
可以从编写者角度理解wrapper和shim这两个概念，wrapper是由消费者编写的封装类，用来封装调用的外部接口以达到自己更好调用的目的，而shim则是提供者编写的封装类，比如你提供了两个版本的API， ver1和ver2,你在ver2上提供了一些新的功能，而现在已经有大量的外部调用者已经调用了ver1，这时你就可以编写一个shim层，在调用ver1基础上转换成调用ver2来达到目的，而ver2也可以同时让新的调用者使用。