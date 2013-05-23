java_binary
============
一个java_binaray规则用来根据它依赖的java_library的编译的.calss和资源文件文件创建一个JAR文件

参数
----

- name（必须）规则的名字，也是产生的jar文件的名字
- deps(默认是[]) 要被编译打包进jar文件的规则（通常是java_library()类型）
- main_class(默认是None)如果提供，它的值就会成为产生的JAR文件的META-INF/MANIFEST.MF的Main-Class属性。同时，当这个规则作为可执行的genrule(),main_class会indicate the class ，哪个main方法会被命令行参数调用。这将成为一个java -jar<name.jar> <args>这样的命令的组成部分

- mainifest_file(默认是None)如果提供，这个manifest会被生成的JAR文件用到，如果它包含了main_class，这个manifest文件会被使用，但是main_classs参数会重载manifest里的main class属性

- visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。
