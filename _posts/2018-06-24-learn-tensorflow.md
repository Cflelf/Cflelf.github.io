---
title:      "TensorFlow自学笔记"
tags:
- TensorFlow
---
> TensorFlow入门记录的一些笔记，以及自己的理解

## 1.TensorFlow的基本概念

Tensor:张量，简单理解为多维数组。

Flow:流，直观表达张量之间通过计算相互转化的过程。

TensorFlow是一个通过计算图的形式来表述计算的编程系统。

计算图:tf.get_default_graph获取默认图，a.graph获得张量a所属的计算图。tf.Graph生成新的计算图，不同计算图上的张量计算不共享。
TensorFlow中的计算图不仅仅可以用来隔离张量和计算，还提供了管理张量和计算的机制

## 2.Tensor张量

张量保存的是如何得到这些数字的计算过程，而不是结果。一个张量主要保存三个属性，名字name，纬度shape，类型type

张量的命名node:src_output

纬度例如shape=(2,),说明是一维数组，长度为2

类型不匹配会报错，类型包括实数，整数，布尔，复数

## 3.Session会话

TensorFlow会话模式有两种，第一种需要明确调用会话生成函数和关闭函数。程序异常退出时会导致资源泄漏

    sess = tf.Session()
    sess.run(...)
    sess.close()

第二种上下文管理器

    with tf.Session() as sess:
        sess.run(...)

TensorFlow不会自动生成默认会话，需要手动指定。eval()函数可以计算张量取值

InteractiveSession可以将会话注册成默认会话

## 4.TensorFlow实现神经网络

训练神经网络的步骤:
(1)定义神经网络的结构和前向传播的输出结果
(2)定义损失函数以及选择反向传播优化的算法
(3)生成会话并且在训练数据上反复运行反向传播优化算法

样例代码如下

    import tensorflow as tf
    from numpy.random import RandomState

    # 神经网络的参数
    w1 = tf.Variable(tf.random_normal([2, 3], stddev=1, seed=1))
    w2 = tf.Variable(tf.random_normal([3, 1], stddev=1, seed=1))

    # 训练数据的大小
    batch_size = 8

    # placeholder的作用是减少常量的定义，用于提供输入数据。shape的一个纬度上使用none可以方便使用不同的batch大小
    x = tf.placeholder(tf.float32, shape=(None, 2), name="x_input")
    y_ = tf.placeholder(tf.float32, shape=(None, 1), name="y_input")

    # 向前传播的过程
    a = tf.matmul(x, w1)
    y = tf.matmul(a, w2)

    # tf.clip_by_value函数可以将一个张量中的数值限制在一个范围内，这样就避免了一些运算错误（比如log0是无效的）。
    cross_entropy = -tf.reduce_mean(y_ * tf.log(tf.clip_by_value(y, 1e-10, 1.0)))
    # 反向传播算法
    train_step = tf.train.AdamOptimizer(0.001).minimize(cross_entropy)

    # 通过随机数生成一个模拟数据集
    rdm = RandomState(1)
    dataset_size = 128
    X = rdm.rand(dataset_size, 2)

    # x1+x2<1为正样本
    Y = [[int(x1 + x2) < 1] for (x1, x2) in X]

    with tf.Session() as sess:
        init_op = tf.global_variables_initializer()
        sess.run(init_op)
        print(sess.run(w1))
        print(sess.run(w2))

        STEPS = 5000
        for i in range(STEPS):
            start = (i * batch_size) % dataset_size  # 每轮选取batch_size个样本训练
            end = min(start + batch_size, dataset_size)

            # 更新参数
            sess.run(train_step, feed_dict={x: X[start:end], y_: Y[start:end]})

            if i % 1000 == 0:
                total_cross_entropy = sess.run(cross_entropy, feed_dict={x: X, y_: Y})

                print("After %d steps, cross entropy is %g" % (i, total_cross_entropy))

        print(sess.run(w1))
        print(sess.run(w2))

