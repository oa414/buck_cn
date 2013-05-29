java_test
============
一个java_test()规则用来定义一系列的用JUnit运行的java测试文件

参数
---

- name （必要） 规则的名字
- srcs(默认是[]) 像java_library(),srcs定义的所有的.java文件在这个规则构建的时候会被编译。此外，所有的用这个规则构建的corresponding .class文件会被传递到Junit运行测试。没有@Test注解的.class文件会被认为是失败的测试，所以请确保仅仅是测试用的类在srcs里面。通常是这样定义srcs的：glob(['**/*Test.java']).
- resource (默认是[]) 和java_libary()相同
- labels(默认是[])一系列这些测试的标签。这些标签是arbitraty text strings，对与buck没有什么意义。但是它们可以，当你运行一个test的时候，一个标签可以定义一个执行buck test的时候的过滤器或者包含的java_test()

- source_under_test(默认是[]) java_test()如果buck测试的java_library()规则。必须是deps参数的子集。如果buck test是用--code-coverage选项执行的话，这个规则的source_under_test的.class文件会被代码覆盖工具EMMA检查 

- visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。
