genfile()
===========

genfile()函数用来定义一个genrule()的输出文件

参数
------

第一个也是唯一一个参数是genrule()或者gen_aidl()规则输出的文件名

例子
--------

下面的例子里面，一个叫messenger_manifest用来在buck-out/gen/目录下生成一个叫AndroidManifest.xml的文件，这个文件是用来作为messager构建规则的messager。路径必须被gemfile函数包括，所以android_binary()用buck-out/gen里面的AndroidManifest.xml而不是构建文件下的那个。

::

	genrule(
	  name = 'messenger_manifest',
	  srcs = [
	    'update_manifest.py',
	    'AndroidManifest.xml',
	  ],
	  cmd = 'python update_manifest.py AndroidManifest.xml > $OUT',
	  out = 'AndroidManifest.xml',
	)

	android_binary(
	  name = 'messenger',
	  manifest = genfile('AndroidManifest.xml'),
	  target = 'Google Inc.:Google APIs:16',
	  keystore_properties = 'keystore.properties',
	  deps = [
	    ':messenger_manifest',
	    # Additional dependent android_library rules would be listed here,
	    # as well.
	  ],
	)

