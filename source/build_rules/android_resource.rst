android_resource
=================

一个android_resource()规则用来打包Android资源文件（传统的是在res和assets目录下）

android_resource()输出是一个通过aa[t -- output-text-symbols 生成的R.txt文件。

参数

- name (必要)规则的名字
- res(默认是[]) Android 资源文件的目录
- pacage(默认是[])这些资源文件生成的R.java的
java包
- assets(默认是[]) 包含Android assets的目录
- deps (默认是[]) 另外的android_resource规则用来通过运行aapt是用-S参数包含
- - visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。

例子

大多数情况下，一个android_resource规则只定义了name, res和package。约定简单的规则一般都叫'res'

android_resource(
  name = 'res',
  res = 'res',
  package = 'com.example',
)