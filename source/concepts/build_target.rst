构建目标
=======

一个构建目标是一个标识一个构建规则的字符串

这里有一个简单的何方的构建目标：

::
	
	//java/com/facebook/share:ui

一个合格的构建目标由三个部分构成：

1. // 前缀表示关于你项目的相对路径。
2. ``java/com/facebook/share`` 部分表示构建文件可以在这个目录下找到
3. :ui表示构建规则的名字，构建规则的名字必须和构建文件一样是唯一的。

一个相对的构建规则可以用来参考另外用同样构建文件的构建目标，一个相关构建规则用:开始，之后之是第三方的组成部分（或者说是短名字，）。比如，参考 ``java/com/facebook/share/BUCK`` ， :ui可以用来参考 ``//java/com/facebook/share:ui:``

::

		# This is in java/com/facebook/share/BUCK.
	  java_binary(
	  name = 'ui_jar',
	  deps = [
	    # This would be the same as using:
	    # '//java/com/facebook/share:ui'
	    ':ui',
	  ],
	)

	
构建目标通常用作构建规则的参数，以及Buck命令行的接口