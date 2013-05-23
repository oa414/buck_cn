下载和安装Buck
================

**注意： Buck只能在Mac OS X和Linux上工作，不支持Windows**

Buck要求先装好以下几个工具：

- Oracle JDK 7
- Ant
- Python 2.7
- Git
- Android SDK


装好这些工具后，你就可以这样安装Buck了

.. code-block:: sh


	git clone git@github.com:facebook/buck.git
	cd buck
	ant
	./bin/buck --help

如果一切顺利，你可以看见这样的东西：

::

		buck build tool
		usage:
		  buck [options]
		  buck command --help
		  buck command [command-options]
		available commands:
		  audit      lists the inputs for the specified target
		  build      builds the specified target
		  clean      deletes any generated files
		  install    builds and installs an APK
		  project    generates project configuration files for an IDE
		  targets    prints the list of buildable targets
		  test       builds and runs the tests for the specified target
		  uninstall  uninstalls an APK
		options:
		 --help         : Shows this screen and exits.
		 --version (-V) : Show version number.


因为你可能经常运行./bin/buck,你应该把它加到你的$PATH环境变量里面，这样就可以从命令行快速启动了。最简单的方法就是在已经加入到$PATH的目录下创建一个符号链接，比如/usr/bin

::

	sudo ln -s ${PWD}/bin/buck /usr/bin/buck

验证是否成功，可以运行which buckl来确认buck是否映射到了/user/bin/buck

