编码规范
===========

写一个完整的Java编程风格指南是一项大工程，所以这里只强调几个重点

- 空格
  - 只用空格，不用tab
  - 标准缩进是2歌空格，继续的行4格空格缩进
  - 每行最多100列
  - 对于方法调用、定义，如果参数一行写不下，那么每个参数都独立一行

- 命名
  - 禁止所有匈牙利命名法
  - 最好用一个字符定义变量，比如catch块里面用e来表示异常，在for语句里用i作为循环变量

- Null
  - 所有引用都是假设为非null的，除了另外说明
  - 用Preconditions.checkNotNull(). ( http://docs.guava-libraries.googlecode.com/git-history/v14.0/javadoc/com/google/common/base/Preconditions.html#checkNotNull(T) )来确认非空参数。
  -一个可能为null的引用应该有@javax.annotation.Nullable.的注解
  - @Nullable的一个好的替代是com.google.common.base.Optional.
  - 注解是可选的

 - API
   - 可以自由的使用Guava库
   - 特别的，请尽量用Guava的不可变的集合。同时为了避免赋值的bug，它们不允许null作为一个元素。
   - 不要用Logger, log4j，用 ``com.facebook.buck.cli.Console`` 来打印日志，并且声明--verbosity标志

 - 注释

   - 注释应该是合格的美语语句，它们应该以大写字母开始，适当的标点结束。
   - 尽量在一个停顿后用一个空格。或许和你高中教的不一样，但是浏览器会把2歌空格压缩为1个，所以习惯吧～
   - 成员的定义应该用javadoc(/** */)来注释，而不是实现中的注释(//)，即使是私有的成员。
   - 一个Javadoc注释应该在一个空行之前。

 - 引入
   - 不要在import中用通配符
   - 没有java的import应该有java的import之前
   - 有java的import应该在有javax的import之前
   - 不要引入无关的import，用你喜欢的IDE来帮助你吧。

 - 警告
   - 代码的编译不应该有警告。为了强制这样，我们在用javac的时候用了-Xlint 和 -Werror 标志，并且用了PMD，http://pmd.sourceforge.net/ 做了静态检查。
   - 警告应该被修正掉，或者用适当的 @SuppressWarnings("type-of-warning")包起来

