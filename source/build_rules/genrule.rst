gen_rule
===========
一个gen_rule()用来从一个shell 命令下产生一个文件。它必须产生一个单独的生成文件

参数

- name(必须)规则的名字
- srcs（必须）在shell 命令下用到的文件列表
- cmd(必须) 用来运行生产输出文件的shell 命令，It will be subject to the following environment variable substitutions:
- 
SRCS
一个space-delimited字符串expansion of srs参数，当每个sercs会被翻译到绝对路径
OUT
genrule()的输出文件，这个变量定义的文件必须总被这个命令写入，如果没有，这条命令的执行会被认为是失败的，构建过程会中断

此外，参考binary build rules（比如java_binary() 或者 python_binary()）可能插入。比如，如果//java/src/com/example:main 指向一个java_library规则，cmd可以是'${//java/src/com/example:main} arg1 arg2 arg3',通过deps里列出的：main
- out（必须）输出文件的名字。必须是唯一的。
- deps(必须)一个在此规则前构建的规则的列表。这必须是一个命令行能处理的大括号包起来的变量 (${//like:this}).
- - - visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。


例子

	这个genrule()用一个Python脚本来从目录树下的AndroidManifest.xml 

	derive一个新的AndroidManifest.xml



	genrule(
  name = 'generate_manifest',
  srcs = [
    'basic_to_full_manifest.py',
    'AndroidManifest.xml',
  ],
  cmd = 'python $SRC_0 $SRC_1 > $OUT',
  out = 'AndroidManifest.xml',
)

参见

genfile() 函数
