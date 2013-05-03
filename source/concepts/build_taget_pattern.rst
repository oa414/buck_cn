构建目标模式
----------

一个构建目标模式是一个用来匹配一个或多个构建目标的描述字符串，它们用在构建规则中给visibility传递参数。

一个构建目标同样是合法的同名的构建目标模式。

::

	# Matches '//apps/myapp:app'.
	'//apps/myapp:app'

一个用冒号结尾的构建目标模式用来匹配同一个目录下的其他规则

::

	# Matches '//apps/myapp:app_debug' and '//apps/myapp:app_release'.
	'//apps/myapp:'

一个用/...结尾的构建规则模式用来匹配在这个构建文件或者这个目录下的构建规则。

::
	
	# Matches '//apps:common' and '//apps/myapp:app'.
	'//apps/...'