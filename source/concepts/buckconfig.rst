buckconfig
==========

你的项目的根目录下或许有一个.buckconfig文件，如果存在的话，Buck会在执行前读取他的内容，这个文件使用INI文件格式。

[alias]
-------
作为 :doc:`../get_start/quick_start` 的一个演示，编译目标的aliases可以定义在.buckconfig里面。


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

它的效果是和在每个构建文件的开头手动调用 ``include_defs('//core/DEFS')`` 是一样的。更多内容请见 include_defs()。




[cache]

这一节定义一个文件系统上，或者开发者共享的分布式的构建缓存，默认是禁用的。注意mode属性决定了其他属性。如果有，就是相关的缓存配置。下面的例子是buck使用了基于目录的缓存，禁用了Cassandra相关的缓存


[cache]
    # Build artifact caching configuration.
    #
    # mode is one of the following:
    #   noop      : No caching (default).
    #   dir       : Use a directory-based cache.
    #   cassandra : Use a distributed Cassandra cache.
    mode = dir

    # Directory path used for directory-based caching. The default directory is
    # buck-cache.
    dir = buck-cache

    # Comma-separated set of known Cassandra cache nodes, for example:
    #
    #   hosts = artifactcache1.example.com, artifactcache2.example.com
    #
    # The default set is empty.
    hosts = localhost

    # Port on which to connect to Cassandra cache nodes. The default port is
    # 9160.
    port = 9160

初始化Cassandra是很直观的，并且按照下面的配置也不复杂

- thrift_framed_transport_size_in_mb 和 thrift_max_message_length_in_mb 默认可能太小了， 取决于最大的构建架构。如果这个设置太小了，大的构建块不会被缓存，并且会收网络吞吐率的影响。
-  scripts/init_cassandra_node.cql 脚本必须运行一次，来创建buck使用的ArtifactCache区块。



[java]
----
这个选项用来定义java代码的根路径，路径可以由逗号隔开。/开头的路径表示和项目根目录的相关路径，这是一个逗号分割的列表，路径是以/（表示项目根目录）的相对路径。比如 

::

  [java]
    src_roots = src, /java/, /javatests/

 会匹配项目根目录下的java和javatests路径。同时也会匹配所有的src文件夹

 注意：这个主要是用来帮助决定是否用一个JAR文件取代java_library()规则的，src_root可能被移去。

 [project]
 -----------

 这一节用来定义default_android_manifest来标识AndroidManifest.xml，如果一个Android规则里project_config()的src_target用到的话。但是构建文件的目录里面不会有AndroidManifest.xml文件。因为一个Android项目的IDE配置文件需要一个AndroidManifest.xml，这提供了一个妥协的方案，防止你的项目用一个样板AndroidManifest.xml文件

::

  [project]
     default_android_manifest = //shared/AndroidManifest.xml



  这一节也可以定义一个initial_targets，是当buck project运行的时候构建目标的空格分割的列表。通常情况下，这是一个genrules的列表，让IDE可以在没有BUCK的时候构建目标。

  比如，如果你有一个用来从Thrife定义文件（ Thrift definition file）产生JAR的genrule，
并且你有一个依赖于此JAR的java_library()规则，你必须确保在把项目导入到Intellij IDLE之前构建genrule()因为Intellij 不知道如何构建genrule()


::

  [project]
    initial_targets = //java/com/facebook/schema:generate_thrift_jar










