buck build
============
构建一个或者多个构建目标。

Buck作为一个构建系统，这是Buck最常用的构建目标。要构建一个构建规则，那就把它的构建目标作为参数穿给buck build:

::

	buck build //java/com/example/app:amazing


Buck会构建这些规则，并且在终端输出产生的文件的路径

参数
-----

- --num-threads Buck应该用来构建的时候用的线程数量。默认是系统处理器数量的1.25倍（在超线程技术的系统上，会把一个核心当作两个用）。

同时你也可以.buckconfig文件里面的build一节里面里定义。默认的处理设置线程数量的顺序是：命令行参数，.buckconfig文件，默认值。实际的线程数量不一定总是与参数一样

- verbose(-v) 来定义输出的日志的详细程度。1最小，10最详细。
  
- build-dependicies (-b) 如何在构建的时候处理依赖，允许的参数值有

   - TRANSITIVE 构建的时候编译所有的依赖
   - WARN_ON_TRANSITIVE 之构建第一级的依赖，如果有传递的依赖的话发出警告
   - FIRST_ORDER_ONLY 只编译第一级的依赖（在它们的build文件的deps里列出的依赖）
