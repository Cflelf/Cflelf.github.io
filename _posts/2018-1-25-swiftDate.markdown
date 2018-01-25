---
title:      "swift时间方面的坑"
tags:
- swift
---

最近在写iOS端的课程表，今天遇到了swift关于时间方面的坑，写下此文记录一下。
首先是关于语言的，我需要得到一周的各星期的简写，swift的方法如下：
    
    let timeFormatter = DateFormatter()
    timeFormatter.dateFormat = "HH:mm:ss E M dd zzz GGG"
    let today = Date()
    print(timeFormatter.string(from: today))
    // 模拟器中星期几是Thu

然后我想那就搞一个星期的数组，["Mon"..."Sun"]，然后得到今天的index，岂不是美滋滋。模拟器上运行的好好的没问题，但是到了我的手机上时，报错了。我一看输出，"周四"，我研究了一下因为我是中文系统，那咋办？如果别人阿拉伯语的手机用我的课程表呢？于是我灵机一动找到了下面的方法：

    let weekdays = timeFormatter.shortWeekdaySymbols
    // 模拟器输出 ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]
    // 我的手机(中文系统)输出 ["周日"....]

苹果还是很机智的，但是很麻烦的一点额，周日在前面，又得写一堆麻烦的把他转成我们中国人习惯的周一在前，而且会出现这个问题：
![是不是很傻][1]


  [1]: /assets/images/blogs/1.jpg
要不我全改成中文算了。。