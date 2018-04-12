---
title:      "iOS面试知识整理"
tags:
- swift
---
> 整理了一些iOS可能面试的点（持续更新）
> 分享一个我最喜欢看的技术博客 [http://www.cnblogs.com/ludashi][1]

## KVO:key-value observer
要求对象支持kvc.常见例子监听scrollview的contentoffset属性，改变控件属性

## runtime
Objective-C 中的方法调用，不是简单的方法调用，而是发送消息
Runtime 系统会把方法调用转化为消息发送，即 objc_msgSend，并且把方法的调用者，和方法选择器，当做参数传递过去.
此时，方法的调用者会通过 isa 指针来找到其所属的类，然后在 cache 或者 methodLists 中查找该方法，找得到就跳到对应的方法去执行。
如果在类中没有找到该方法，则通过 super_class 往上一级超类查找

## ARC，MRC
ARC：auto reference counting ，继承自NSObject的类
MRC：手动release，retain
auto release pool：放到自动释放池的对象是在超出自动释放池作用域后立即释放的。autorelease方法会把对象存储到AutoreleasePoolPage的链表里。等到auto release pool被释放的时候，把链表内存储的对象删除。

## Strong Weak unowned
强引用使引用对象的引用计数+1，避免对象被ARC机制销毁。本质上来讲，任何对象只要有强引用，它就不会被销毁掉
弱引用不会使引用对象的引用计数+1。弱引用的对象被销毁后，弱引用的指针会被清空。这样保证了当你调用一个弱引用对象时，你能得到一个对象或者nil. 
unowned引用是non-zeroing(非零的), 在ARC销毁内存后，不会被赋为nil, 这表示着当一个对象被销毁时, 它指引的对象不会清零. 也就是说使用unowned引用在某些情况下可能导致dangling pointers(野指针). 所以在访问无主引用的时候，要确保其引用正确，不然会引起内存崩溃.

在引用对象的生命周期内，如果它可能为nil，那么就用weak引用。反之，当你知道引用对象在初始化后永远都不会为nil就用unowned. 

当你声明一个属性时,它默认就是强引用。弱引用对象是必须用var关键字, 不能用let.

## Optional
普通的可选类型用 '?' 来声明，表示该值可能存在可能为nil。相当于声明了一个包含 Optional.None和Optional.Some(T)的枚举类型。
隐式拆包用 '!' 来声明，表示确认该值不为nil，一定存在。相当于声明了Optional.Some(T)。

解包两种方式，一种判断是否为nil，再用强制解包。另一种optional binding，if let

Optional chaining 链式访问 例子if let isPNG = imagePaths[“star"]?.hasSuffix(".png")

??的使用.当optional解包后值为nil时，提供默认值

## 闭包
形式： { (parameters:Type) -> returnType in
		stats
	}
值捕获：捕获函数体内的值
逃逸闭包：一般用于异步函数的回调，比如网络请求成功的回调和失败的回调。语法：在函数的闭包行参前加关键字“@escaping”。
逃逸闭包的2个场景：
	异步调用: 如果需要调度队列中异步调用闭包， 这个队列会持有闭包的引用，至于什么时候调用闭包，或闭包什么时候运行结束都是不可预知的。
	存储: 需要存储闭包作为属性，全局变量或其他类型做稍后使用。

## Alamofire原理：对NSURLSession的封装
几个核心文件
Alamofire.swift:提供给用户的接口，调用Manager中的方法.URLStringConvertible协议,将各种url转换成string
SessionManager.swift: 单例，定义了session对象，创建Request对象发送请求，管理request对象
Request.swift：根据manager传来的session对象，任务初始化
Response.swift


## NSURLSession
三种类型
Default默认：磁盘缓存，将证书存入钥匙串
Ephemeral临时：存在RAM中
Background后台：使用单独的线程处理会话，其他与默认一致

三种任务：数据任务，下载任务，上传任务
功能：URL编码，将get请求的参数拼接到url

缓存策略
1.使用NSMutableURLRequest指定缓存策略
2. 使用NSURLSessionConfiguration指定缓存策略
3.使用URLCache + request进行缓存
4.使用URLCache + NSURLSessionConfiguration进行缓存

https请求认证：回掉NSURLSessionDelegate中的didReceiveChallenge，从保护空间取出认证方式。

  [1]: http://www.cnblogs.com/ludashi
