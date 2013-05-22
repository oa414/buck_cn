一个java_library()规则用来定义一系列的可以编译到一起的java文件，java_library规则的主要输出是一个包含所有编译的类文件和资源文件的JAR包

参数

name (必要)规则的名字
srcs(默认是[])这个规则编译的java文件集合

resource 默认是[] 包含在已编译的.class文件里的静态文件。这些文件可以用Class.getResource进行装载

注意 buck用buckconfig.html里的src_roots来帮助确定哪里的resource应该被放置到生成的jar文件里面

- deps( 默认是[])规则。（通常是另外一个java_library）用来生成编译这个java_library需要的classpath
- source(默认是'6') 编译用的Java语言版本。Corresponds to source argument for javac.
- target (默认是'6') 默认编译的字节码版本。Corresponds to the -target argument for javac.
- export_deps(默认是False)Whether or not depending on this rule should also transitively pull in all of its dependencies.
- - - visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。


例子
---


# A rule that compiles a single .java file.
java_library(
  name = 'JsonUtil',
  srcs = ['JsonUtil.java'],
  deps = [
    '//third_party/guava:guava',
    '//third_party/jackson:jackson',
  ],
)

# A rule that compiles all of the .java files under the directory in
# which the rule is defined using glob(). It also excludes an
# individual file that may has additional dependencies, so it is
# compiled by a separate rule.
java_library(
  name = 'messenger',
  srcs = glob(['**/*.java'], excludes = ['MessengerModule.java']),
  deps = [
    '//src/com/facebook/base:base',
    '//third_party/guava:guava',
  ],
)

java_library(
  name = 'MessengerModule',
  srcs = ['MessengerModule.java'],
  deps = [
    '//src/com/facebook/base:base',
    '//src/com/google/inject:inject',
    '//third_party/guava:guava',
    '//third_party/jsr-330:jsr-330',
  ],
)

# A rule that builds a library with both relative and
# fully-qualified deps.
java_library(
  name = 'testutil',
  srcs = glob(['tests/**/*.java'], excludes = 'tests/**/*Test.java'),
  deps = [
    ':lib-fb4a',
    '//java/com/facebook/base:base',
  ],
)