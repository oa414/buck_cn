一个gen_aidl文件用来从.aidl文件生产.java文件。它不久之后就会被废弃，其功能会被android_libray吸收。

参数

- name（必须） 规则的名字
- aidl(必须) 需要转换成.java文件的aidl文件的路径
- import_path(必须)引入对aidl声明的搜索路径（相当于- I 参数，当从命令行调用aidl的时候）
- deps(默认是[]) 一个必须在此规则构建之前构建的规则的列表
- visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。
