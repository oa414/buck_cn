buck clean
=============

删除Buck产生的所有文件

Buck会追踪所有可以交付的文件，所以删除这些文件可以保证Buck回到clean build的状态，Buck存储的文件可能发送了改变，但是buck clean总是清除掉当前的版本的文件。

参数
---

- --verbose (-v) 来定义输出的日志的详细程度。1最小，10最详细。