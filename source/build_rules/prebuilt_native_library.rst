prebuilt_native_library
========================

一个prebuilt_native_library规则用来为Android打包native 库（比如.so文件）

参数
----

- 
name （必须） 规则的名字
- native_libs (默认是None) 包含native 库的目录路径。这个目录对于不同的
- CPU架构有不同的字母里，比如armeabi和armeabi-v7a
- - visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。


例子

---
大多数时间，一个prebuilt_native_library相对于使用它的android_library是私有的


prebuilt_native_library(
  name = 'native_libs',
  native_libs = 'libs',
)

android_library(
  name = 'my_lib',
  srcs = glob(['*.java']),
  deps = [
    ':native_libs',
  ],
)