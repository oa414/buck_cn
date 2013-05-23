解决问题
======

如果Buck停止工作，你可以试试以下方法

确保你使用的是Oracle JDK
---------------------
Buck只用Oracle JDK测试过，如果你用OpenJDK，不能保证能正常工作

重新构建Buck
---------------
在Buck短暂的历史上，自动升级的逻辑导致过一些Bug。如果你认为问题是因为Buck自动升级导致的，你最好自己手动构建一下Buck。

::

	cd <directory-where-you-checked-out-Buck>
	git checkout master
	git pull --rebase
	ant clean jar


运行Buck clean
-------------

理想情况下，这个方案是从来不会用的。因为Buck如果正常工作的话，它会知道那些文件是修改过，那些需要重新构建。

即便如此，Buck是不完美的，所以你会发现一些缺陷的地方。在这种情况下，用buck clean来试试？

确保你没有一个.nobuckcheck 文件
----------------------

如果你曾经在Buck项目上工作，你会建立一个.nobuckcheck文件来禁止Buck自动升级的特性。当你完成修改的时候，你应该删除.nobuckcheck文件来让Buck自动升级。

如果你忘了做这个，如果有些人需要用更新的Buck来完成你的项目，而且你本地的buck版本又是过时了的，那就定期运行git pull --rebase来手动检出Buck的最新版本。

删除你项目里面的所有生成的文件
------------------------

Buck所有产生的文件都会写入到buck-out目录，可以让buck clean简单的实现。然而，你可能会用其他工具（比如IDE）来在目录树下产生输出文件，这些文件可能无意的被glob()规则包含，从而影响Buck。

比如，如果你用Git，你运行

::

	git clean -xfdn

来列出你目录下没有被包含到版本控制系统里面的文件，-n表示仅仅运行，让Git不要删除那些文件。如果你希望Git删除掉没有包含金版本控制系统里面的文件，那么用-e选项：

::

	git clean -xfd -e local.properties

注意-e可以被多次定义。Note that -e can be specified multiple times.


