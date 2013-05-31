buck project
===============
产生IDE使用的配置文件

现在，只支持Intellij

这个命令处理所有BUCK文件的 :doc:`../build_rules/project_config` 规则并且用它们产生IDE的配置文件。

对于Intellij，产生的文件包含

- .idea/libraries/*.xml 每一个定义了一个库，一个库总是符合prebuilt_jar
- .iml 每一个定义了一个intellij的module。 每个目录下的BUCK文件有project_config的话，就会生成一个iml文件。一个module可以依赖另外一个moudle。就像礼物一个库。
- .idea/modules.xml 列出了intellij 里面所有的module。

因此，运行buck project会建立以上所有文件，不像其他的Buck命令产生的输出，会被buck clean清理掉。

注意所有buck project产生的文件应该被列入.gitignore

参数
---
无
