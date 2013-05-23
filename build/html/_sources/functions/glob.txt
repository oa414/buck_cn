glob()
======

glob()函数用来确定用模式匹配的一些列文件

参数
------

- 第一个参数是一个字符串列表，定义了在当前目录下要匹配的文件名的模式
- excludes列表，（默认是[]），列表里的模式匹配的文件名会被排除。

例子
----

所有当前目录下的Java文件

::

	glob(['*.java'])

所有当前目录下的Java和aidl文件

::

	glob(['*.java', '*.aidl'])


所有在当前目录下的Java文件

::

	glob(['**/*.java'])


所用在当前目录下用Test.java结尾的文件

::

	glob(['**/*Test.java'])

所用在当前目录下用Test.java结尾的文件，以及StringTests.java文件

::

	glob(['**/*Test.java', 'StringTests.java'])


所用在当前目录下用Test.java结尾的文件，除了HaltingProblemTest.java文件

::

	glob(['**/*Test.java'], excludes = ['HaltingProblemTest.java'])


所有在当前目录下的Java文件，除了用Test.java结尾的文件

::

	glob(['**/*.java'], excludes = ['**/*Test.java'])
