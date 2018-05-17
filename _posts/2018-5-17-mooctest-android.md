---
title:      "Mooctest移动应用测试mac环境配置"
tags:
- mooctest
---
这次测试真的巨坑，对mac用户十分不友好，我花了大概一个下午踩坑才终于完成了配置，成功提交了.

废话不多说了，首先下载Appium，这里我推荐下载客户端版的，省的配置，下载地址 [https://github.com/appium/appium-desktop/releases/tag/v1.6.1][1]下载Appium-1.6.1.dmg即可,下载完成后直接运行启动服务器即可

接着配置安卓环境，去官网下载Android SDK. [http://www.androiddevtools.cn][2]下载24.4.1版本的android-sdk_r24.2.macosx.zip

下载完成后解压，放到/usr/local,地址其实无所谓.然后配置环境变量$ANDROID_HOME. 在Terminal下输入 open ~/.bash_profile,在文件中补充

    export ANDROID_HOME=/usr/local/android-sdk-macosx
    export PATH=$PATH:$ANDROID_HOME/tools
    export PATH=$PATH:$ANDROID_HOME/platform-tools

保存后输入 source ~/.bash_profile应用刚刚的设置。

接着在命令行中输入adb version如果没有出现command not found，应该就配置好了安卓环境.否则echo $ANDROID_HOME检查一下你的环境变量

接下来是模拟器的下载，我选择使用Genymotion，下载地址[https://www.genymotion.com][3] 需要注册后下载，我下的是试用版只有30天，但足够撑到考试结束。下完后运行，我的电脑没有virtual box，因此还需要下载[https://www.virtualbox.org/wiki/Downloads][4]

下载完成后运行Genymotion，添加一个模拟器，我加的安卓版本是4.4.4，因为mooctest里的脚本用的安卓版本是4.4，省的改了，打开后在命令行输入adb devices检查一下是否有模拟器。

到此为止就可以去mooctest运行代码了，附上成功的图片。如果有问题可以试一下appium-doctor，把他所有诊断出的错误信息完成即可，具体配置网上查吧，我也不太记得了。。
![此处输入图片的描述][5]



    
    


  [1]: https://github.com/appium/appium-desktop/releases/tag/v1.6.1
  [2]: http://www.androiddevtools.cn
  [3]: https://www.genymotion.com
  [4]: https://www.virtualbox.org/wiki/Downloads
  [5]: /assets/images/blogs/2.jpeg
