列出当前项目下可用的buck target

下面的命令会打印所有build targetsd(按字母序)到标准输出

这个命令可以手动指定特定的项目，比如在//java/com/myproject运行所有的java测试。

::

	buck targets --type java_test | \
  	grep '//java/com/myproject' | \
  	xargs buck test

 可以传递给buck targets一系列规则，Buck会只打印出符合规则的targets

 ::

 	buck targets --show_output //java/com/myproject:binary
	> //java/com/myproject:binary buck-gen/java/com/myproject/binary.apk

参数
----
--type 过滤target的类型，如

buck targets --type java_test java_binary

--referenced_file 过滤依赖特定文件的targets

比如，一个开发者想要运行所有被特定文件影响到的targets，可以这样

buck targets --type java_test \
  --referenced_file java/com/example/Foo.java |
  xargs buck test

 -- json 打印每个target的json格式的信息

 -- resolvealias 打印所有关联到特定别名的build target的信息，这个命令同样可以用build targets作为参数。参见.buckconfig

 -- show_output 对于每个规则，在规则名字后面打印相关的输出的目录