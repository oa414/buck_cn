android_library
==================
android_library目标用来定义一系列的java文件。android_library规则的输出目标是一个包含了以及编译的class文件和资源文件的jar包

参数
----

- name （必须） 规则的名字

- src （默认是[]） 这个规则编译的java文件
- resource 默认是[] 包含在已编译的.class文件里的静态文件。这些文件可以用Class.getResource进行装载

注意 buck用buckconfig.html里的src_roots来帮助确定哪里的resource应该被放置到生成的jar文件里面

- deps( 默认是[])规则。（通常是另外一个android_library）用来生成编译这个android_library需要的classpath

例子
----

一个android_library规则以及一个android_resource规则。这是一个http://developer.android.com/tools/projects/index.html定义的通常情况下android库项目


::

	android_resource(
	  name = 'res',
	  res = 'res',
	  package = 'com.example',
	)

	android_library(
	  name = 'my_library',
	  srcs = glob(['src/**/*.java']),
	  deps = [
	    ':res',
	  ],
	)