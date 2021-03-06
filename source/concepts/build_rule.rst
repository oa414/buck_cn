构建规则
======

一个构建规则是从一系列输入产生输出的处理流程。

Buck有一些内建的常用的构建Android程序的构建规则。例如，基于Andorid SDK编译Java代码是最常用的操作，所以存在android_library来帮助做这些事情。类似的，许多Android开发者都要创建APK，所以android_library可以干这个。

构建规则用python函数描述在构建文件中。（Python函数定义了规则，然后相对应的Java对象根据这些规则进行处理，虽然技术上是不同的东西）、每个构建规则至少有三个下面的参数：

- name 构建规则的名字，必须是构建文件里面唯一的
- deps 构建规则的依赖，是一个构建目标的列表
- visibility 允许是否允许这个构建规则成为一个依赖，表达为一个构建目标模式的列表

在Buck里面，每个构建规则可以产生0个或者1个输出文件，输出文件可以用来为其他构建规则作为依赖。比如，android_library的输出是一个JAR文件，所以 :doc:`../build_rules/android_binary` 定义 :doc:`../build_rules/android_library` 作为它的依赖，它就可以在其APK包里包含android_library输出JAR包里面的class文件。如果一个android_library依赖另外一个，它的依赖规则里面的JAR会在编译的时候被包含到classpath中。具体可以看文档。使用deps没有什么问题，所有的规则的deps会保证在其之前构建。

构建规则和它们的依赖定义了一个有向图，Buck需要它是无环的。这可以让Buck更有效的进行构建工作，并且可以让独立的子图进行并行的构建。


虽然Buck内建了很多规则，但是仍然不能面面俱到。所以Buck有一个逃生舱——提供了一个vanilla构建规则，叫genrule，可以用来用Bash脚步构建任何文件。

最后，注意构建文件是被当作Python文件求值的，这意味这你可以在你的构建文件里面定义自己的函数，你可能不常用到这个功能，但是如果你希望在构建的时候自定义一些东西的话会很方便，而不用去改Buck的源代码。更多的内容见macros。

