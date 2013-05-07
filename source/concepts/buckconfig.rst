buckconfig
==========

你的项目的根目录下或许有一个.buckconfig文件，如果存在的话，Buck会在执行前读取他的内容，这个文件使用INI文件格式。

[alias]
-------
作为Quick Start的一个演示，编译目标的aliases可以定义在.buckconfig里面。


::

	[alias]
	  app     = //apps/myapp:app
	  apptest = //apps/myapp:test

::

	这可以让命令行这样用

::

	buck build app
	buck test apptest


[buildfile]
----------
这个选项可以定义一每个构建文件都会自动包含的构建文件

::

	[buildfile]
	  includes = //core/DEFS

它的效果是和在每个构建文件的开头手动调用include_defs('//core/DEFS')是一样的。更多内容请见 include_defs()。

[java]
----
这个选项用来定义java代码的根路径，路径可以由逗号隔开。/开头的路径表示和项目根目录的相关路径，