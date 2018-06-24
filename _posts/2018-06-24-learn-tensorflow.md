---
title:      "TensorFlow自学笔记"
tags:
- TensorFlow
---
> TensorFlow入门记录的一些笔记，以及自己的理解

#1.TensorFlow的基本概念
Tensor:张量，简单理解为多维数组。

Flow:流，直观表达张量之间通过计算相互转化的过程。

TensorFlow是一个通过计算图的形式来表述计算的编程系统。

计算图:tf.get_default_graph获取默认图，a.graph获得张量a所属的计算图。tf.Graph生成新的计算图，不同计算图上的张量计算不共享。
TensorFlow中的计算图不仅仅可以用来隔离张量和计算，还提供了管理张量和计算的机制

#2.Tensor张量
张量保存的是如何得到这些数字的计算过程，而不是结果。一个张量主要保存三个属性，名字name，纬度shape，类型type

张量的命名node:src_output

纬度例如shape=(2,),说明是一维数组，长度为2

类型不匹配会报错，类型包括实数，整数，布尔，复数

#3.Session会话
TensorFlow会话模式有两种，第一种需要明确调用会话生成函数和关闭函数。程序异常退出时会导致资源泄漏

    sess = tf.Session()
    sess.run(...)
    sess.close()

第二种上下文管理器

    with tf.Session() as sess:
        sess.run(...)

TensorFlow不会自动生成默认会话，需要手动指定。eval()函数可以计算张量取值

InteractiveSession可以将会话注册成默认会话

#4.TensorFlow实现神经网络
明天写