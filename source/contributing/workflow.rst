工作方式
========

在  :doc:`../get_start/install` 里介绍的，你可以在github检出buck后用ant构建buck。如果你改变了Buck的源文件，你可以用ant complie来重新构建。如果你觉得Buck状态有问题，你可以用 ``ant clean && ant``  来清楚构建

实际上，没有一个特别快的开发周期。不过我们发现用Eclipese开发会很快，Buck包含了Eclipese的.classpath和.project文件，所以buck可以被Eclipese以一个java项目倒入。这个项目已经被配置为当你保存你代码的时候，Eclipese自动重写buck期望的.class文件（加入你开启了Eclipese的自动构建功能）

这意味着每当你在Eclipse保存代码的时候，你可以切换到命令行来运行Buck。如果你的Eclipese出了问题，运行Project->Clean，然后运行File->Refresh,应该能解决。


注意，如果你在开发Buck，你应该建立一个.nobuckcheck在项目根目录下。更多请见 :doc:`../concept/nobuckeck` 和 :doc:`../concept/buckversion` 

注意：Buck包含了一些可以作为一个Porject被import到Intellij 的元文件。用Intellij 开发Buck也很愉快，但是，他不能像Eclipese一样重写当前的class文件（你可以建立一些构建步骤来让Intelij来支持它，但是我们没有）。所以，用Intellij开发Buck的时候，你应该为buck起一个alias，让ant complie在buck之前运行。