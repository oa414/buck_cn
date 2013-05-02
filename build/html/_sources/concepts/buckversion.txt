buck版本
========


如果你的项目的根目录下包含了一个叫做.buckversion的文件，Buck会期望它包含了一个Git的Hash值来确定用来构建你的项目的版本。

如果.bucmversion 存在，bin/buck会比较。buckversion和你检出的buck的git rev-parse HEAD的值，如果值不同，buck会自我升级到你指定的版本（使用Git fetch和Git checkout）并且自我构建。

如果.buckversion不存在，Buck假设是在正确的版本环境下，在这种场景，bin/buck只有在找不到编译好的class文件的时候的时候重新构建buck，

在Facebook，buck经历了巨大的变化，所以每当buck升级的时候，使用buck的项目对应的构建可能就会变得混乱。所以.buckversion让项目可以自由的脱离buck本身的发展。

如果想取消这个特性，你可以这样：

删除.buckversion文件

或者增加一个.nobuckcheck文件，更多文件请看.nobuckcheck

注意，.buckversion的Git hash是Facebook在Github上发布的仓库的。如果你fork了buck，这个模式只适用于你fork的仓库了。

