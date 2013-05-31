build test
==============
构建并且运行制定目标的测试

::
	
	buck test //javatests/com/example:tests

参数
-----

--all 如果有该参数，Buck会运行目录树下所有测试
--code-coverage  当运行测试的时候收集代码覆盖程度。当前，只能用`EMMA <http://emma.sourceforge.net/>`_ 来执行。在运行

::

	buck test --all --code-coverage

代码覆盖率的信息可以在buck-out/gen/emma/coverage/
找到

--debug 如果有该参数，测试会开始悬挂在一个JDWP调试的5005端口，这意味着有一个调试器连接上的时候测试才开始。

--include (-i) 运行测试的标签。标签是一种把测试组织在一起，或者一起运行特定类型测试的一个方法。比如，一个开发人员可以用fast标签标记所有运行少于100毫秒的测试，使用

::

	buck test --all --include fast

来运行所有快速的测试。更多内容请见 :doc:`../build_rules/java_test`



--exclude (-e) 和include相反，定义的标签的测试不会被运行。比如，如果我们希望运行所有测试，除了那些慢的，我们可以运行

::

	buck test --all --exclude slow

- --num-threads Buck应该用来构建的时候用的线程数量。默认是系统处理器数量的1.25倍（在超线程技术的系统上，会把一个核心当作两个用）。

同时你也可以.buckconfig文件里面的build一节里面里定义。默认的处理设置线程数量的顺序是：命令行参数，.buckconfig文件，默认值。实际的线程数量不一定总是与参数一样


- verbose(-v) 来定义输出的日志的详细程度。1最小，10最详细。

- --xml 如果定义，buck会在制定的xml文件输出测试结果，比如

::

	buck test --all --xml testOutput.xml

