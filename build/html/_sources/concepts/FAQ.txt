FAQ
=====


Q: 为什么Buck不支持Windows。

A: 要让Buck支持Windows，有很多工作还要做：

- Buck在很多地方用了软链接，我们需要在很多地方替换成Windows下等价的东西
- Buck硬编码了OS X/Linux的文件和路径分隔符，这或许是一个不好的软件设计，但是可以让代码在很多情况更具可读性，尤其是单元测试的时候
- 要增加很多文档维护的工作。比如说，一些OS X/Linux的小技巧，比如

::

	buck targets --type java_test | xargs buck test

在Windows下不能工作，所以必须要有单独的文档

- ./bin/buck是Bash写的，为了支持Windows必须有相应的Windows批处理脚步，同时保持两个脚本的工作很烦。当然，我们可以用跨平台的预言，比如Ruby，但是要用户安装额外的工具。

- 要增加测试的地方。

虽然有那么多额外的活，我们还是希望未来能支持Windows。

Q：为么么Buck用Ant构建而不是Buck？

A: 自举的系统将会更难维护和调试。

如果Buck用Buck自己构建，每当Buck的源代码提交的时候，应当包括一个Buck的二进制执行文件。这件事很容易被忘掉，所以很难用正确的二进制文件来验证。用Ant编译Buck可以确保我们用源码构建，可以更容易验证。

另外，因为Ant比Buck成熟，所以它支持许多我们还没有在Buck里面实现的特性，比如生成Javadoc，用PMD进行静态分析，Python单元测试等。

即使如此，Buck也可以用自己来构建。当你用Ant构建Buck之后，可以运行./bin/buck build buck来重新构建Buck。
