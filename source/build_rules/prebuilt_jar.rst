一个prebuilt_jar()规则用来定义一个我们仓库种作为预编译的二进制文件的jar包，而不是用Buck构建的代码。通常情况下，这个规则用来指向一个第三方的jar文件（比如junit.jar）并且用作java_library()规则的依赖

参数
---
name （必须） 规则的名字
binary_jar(必须) 预编译jar文件的路径
source_jar(默认是None) binnary_jar里面对应的.java文件的jar包。这通常用来debug。
javadoc_url（默认是None） binary_jar里的.class文件对应的javadoc的URL
-deps (默认是[]) 必须在这个规则之前构建的规则。因为binary_jar以及构建了，所以应该没有需要构建的短信，这个应该是空的。
 - - visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。
 - 
 例子

 prebuilt_jar(
  name = 'junit',
  binary_jar = 'junit-4.8.2.jar',
  source_jar = 'junit-4.8.2-sources.jar',
  javadoc_url = 'http://kentbeck.github.com/junit/javadoc/4.8/',
)

java_library(
  name = 'tests',
  srcs = glob(['tests/**/*Test.java']),
  deps = [
    ':junit',
  ],
)
