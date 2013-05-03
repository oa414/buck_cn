可见度
=====

可见度来表示一个构建规则是否可以在构建依赖中被包含到一个构建目标里面。在大项目中，你可能希望阻止开发人员来给一些项目改动代码。减少构建规则的可见度可以帮助组织这个行为。

构建规则默认可以让在同一个构建文件里面的构建目标看见。

如果耀荣构建规则被其他构建目标看见，在构建规则里面增加visibility参数并提供一个构建目标列表。

同时有一个特殊的规则：PUBLIC，可以让构建规则对其他所有构建规则可见。

例子
----

一个像Guava的库通常可以让所有的构建规则访问。

::

	prebuilt_jar(
	  name = 'guava',
	  binary_jar = 'guava-14.0.1.jar',
	  visibility = [
	    'PUBLIC',
	  ],
	)


常用的一个地方是让Android资源文件只对用到它的Java代码可见

::

	android_resource(
	  name = 'ui_res',
	  res = 'res',
	  package = 'com.example',
	  visibility = [
	    '//java/com/example/ui:ui',
	  ],
	)

或者更简单的让整个目录可见，在java/com/example/ui/BUCK:增加一个构建规则

::

	android_resource(
	  name = 'ui_res',
	  res = 'res',
	  package = 'com.example',
	  visibility = [
	    '//java/com/example/ui:',
	  ],
	)

最后，经常限制那些为了测试用代码仅对测试可见。如果你在项目根目录下的javatestes/下定一个你的Java单元测试，你可以定义如下的规则，来让javatest/可以依赖于JUnit

::

	prebuilt_jar(
	  name = 'junit',
	  binary_jar = 'junit-4.11.jar',
	  visibility = [
	    '//javatests/...',
	  ],
	)
