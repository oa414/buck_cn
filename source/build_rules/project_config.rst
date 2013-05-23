project_config
================
一个project_config规则用来定义生产IDE哦诶之文件的信息，让IDE也可以构建BUCK文件

注意每个BUCK文件只有一个project_config()规则

参数
-----

- src_target(必要)一个构建文件会 以它为基础构建项目。它必须是java_library(), java_test(), android_library(), or android_binary(). 中的一个。
生产的IDE项目会成为对应规则的类型。

此外，这个规则的deps会确定这个规则生产的IDE项目的其他IDE项目依赖。注意这是启发式的，所以实际上可以正确运行。

- src_roots（默认是[]） src_target对应源代码的根目录。值可以是
- Node 没有源码目录。在src_target是一个没有java代码的Android 资源库项目的时候肯呢个是这样
- []包含了一个Java包的目录。在这种情况下，包文件的根目录必须是其中一个祖先目录。Buck可以通过.buckconfig的src_roots属性的java段来推导项目的根目录。
- ['src'] 构建文件目录下的源码目录，这是一个有多个元素的列表，但是实际上，它应该总是包含了最多一个元素。选项支持多个元素是为了检查导入的传递性。在这种情况下，src_roots会是['src', 'src-gen'].



- test_target (默认是None) 定义项目的测试项目。在Intellij里，即使classpaths不同，源代码和测试代码也可以在同一个moudle里，在Elipese，源代码和测试代码必须在不同的目录里，因为classpath不同

- test_roots (默认是 []) 等同于 src_roots, 但是目标是test_target.





例子
===

::

	project_config(
	  src_target = ':lib-base',
	  src_roots = [ 'src' ],
	  test_target = ':tests',
	  test_root = 'tests',
	)