buck文件
=========

你的项目里的构建文件只有是最靠近你项目的文件树的直接祖先的时候，才被当作构建文件处理。比如，在你的项目里有以下构建文件：

::

  java/com/facebook/base/BUCK
  java/com/facebook/common/BUCK
  java/com/facebook/common/collect/BUCK

你的构建文件会有以下约束

 ``java/com/facebook/base/BUCK`` 的规则只对 ``java/com/facebook/base/`` 下的文件有效
 ``java/com/facebook/common/`` 对目录下除了 ``java/com/facebook/common/collect`` 的所有文件有效

这些源文件也被叫做构建包。

如果要跨构建包访问一些代码，办法是在构建规则里面建立deps，指向其他代码。回到签名的例子，假如 ``java/com/facebook/common/concurrent/ ``的代码依赖于 ``java/com/facebook/common/collect/`` 的代码，假设 ``java/com/facebook/common/collect/BUCK`` 有一个这样的构建规则：

::

  java_library(
    name = 'collect',
    srcs = glob(['*.java']),
    deps = [
      '//java/com/facebook/base:base',
    ],
  )


java/com/facebook/common/BUCK 可以有这样一个构建规则：

::

    java_library(
      name = 'concurrent',
      srcs = glob(['concurrent/*.java']),
      deps = [
        '//java/com/facebook/base:base',
        '//java/com/facebook/common/collect:collect',
      ],
    )



但是下面的例子是非法的。因为 ``java/com/facebook/common/collect/`` 有自己的构建文件，所以不能把 ``java/com/facebook/common/collect/*.java`` 作为它的源文件。

::

    java_library(
      name = 'concurrent',
      srcs = glob(['collect/*.java', 'concurrent/*.java']),
      deps = [
        '//java/com/facebook/base:base',
      ],
    )