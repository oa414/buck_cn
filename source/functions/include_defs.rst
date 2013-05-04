include_defs()
==============

include_defs()函数用来韩寒来自其他文件的宏和常量

include_defs()函数用来执行当前构建文件上下文里面的构建文件风格的代码。此外，那些代码可能用了一些Buck函数，比如java_library(),java_test()等待，甚至include_defs自己！

include_defs()的意图是避免在构建文件里复制粘贴代码。通常，包含的文件里面会包括书籍定义（见下面的例子），或者宏定义，以创建更复杂的构建规则。

参数
--------

唯一的参数是一个路径，指向一个包含宏和常量的文件。它看起来像一个构建目标，因为它以//开头（用来指向项目的根目录），但是它不是一个正确的构建规则，因为它的文件依赖指向的是项目的根目录，而不是构建规则。

例子
-------

假如文件core/DEFS有以下内容

:: 

	JARS_TO_EXCLUDE_FROM_DX = [
	  'third_party/guava/guava-14.0.1.jar',
	  'third_party/jackson/jackson-core-2.0.5.jar',
	  'third_party/jackson/jackson-databind-2.0.5.jar',
	  'third_party/jackson/jackson-datatype-guava-2.0.4.jar',
	]

另外的构建文件可以用 include_defs来包含这个数组，以避免复制粘贴操作。

::

	include_defs('//core/DEFS')

	android_binary(
	  name = 'example',
	  # ...
	  no_dx = JARS_TO_EXCLUDE_FROM_DX,
	)