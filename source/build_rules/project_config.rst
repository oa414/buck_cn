一个project_config规则用来定义生产IDE哦诶之文件的信息，让IDE也可以构建BUCK文件

注意每个BUCK文件只有一个project_config()规则

参数

src_target(必要)一个构建文件会 will serve as the basis of the generated project。它必须是java_library(), java_test(), android_library(), or android_binary(). 中的一个。
生产的IDE项目会成为对应规则的类型。

Also, the deps of this rule will determine the other IDE projects on which the IDE project for this rule depends. Note that this is a heuristic, but it appears to work reasonably well, in practice.

src_roots（默认是[]） src_target对应源代码的根目录。值可以是
- None 没有源码根目录。在src_target定义一个Android_Library项目是纯资源项目，没有
- 
Java代码的是可以是这样

- []The directory that contains the build file is a Java package. In this case, the root of the package must be one of the ancestor directories. Buck can deduce where the root of the package is by using the src_roots property in the [java] section of the .buckconfig.
- ['src'] The src directory under the build file directory is a source root. This list may contain multiple elements, but in practice, it should almost always contain at most one element. This option supports a list to allow generated source code to be checked in alongside the hand-written source of imported projects. In this case, src_roots would be ['src', 'src-gen'].
- 
test_target (defaults to None) If specified, a complementary tests project for src_target. In IntelliJ, source and test code can be grouped together in the same module while having different classpaths. In Eclipse, source and test code are often in separate projects because they need to have different classpaths.

test_roots (defaults to []) Same as src_roots, but for test_target.

例子
===
project_config(
  src_target = ':lib-base',
  src_roots = [ 'src' ],
  test_target = ':tests',
  test_root = 'tests',
)