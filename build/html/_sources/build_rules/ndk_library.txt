ndk_library
=============
一个ndk_library用来定义一系列C/C++文件，一个Android.mk和一个Application.mk文件用来为NDK里的ndk-build工具产生一个或更多的共享对象

参数
---

- name （必须） 规则的名字
- deps (默认是[]) 这个规则构建之前需要构建的规则
- flags(默认是[]) 一个字符串数组passed verbatim to ndk-build。 通常这个是不需要的。但是在一些情况下你可能想要在这个地方放一些短信。比如，这个可以用来构建debug mode的库（NDK_DEBUG=1）或者建立the number of jobs spawned by nkd-build(默认青龙峡 是和CPI核心数相同)


- visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。

一个包含这个library的android_binary()将在APK根目录下的lib目录下生成所有的native 共享对象。

不像默认的ndk-build调用，buck会将所有的intermediate文件和构建的输出放到一个buck-out/gen的子目录下

另外，因为ndk-build和Android.mk没有暴露必要的依赖信息，buck会假设所有的下列扩展名的文件是要构建的c, cpp, cc, cxx, h, hpp, mk.