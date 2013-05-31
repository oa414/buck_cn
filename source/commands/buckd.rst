buckd
==========

开启一个buck守护进程

::

	buckd


(因为Buck也在不断开发中，官方文档也在变化，这篇后来才出来的文档，所以暂时没有翻译。)


This command starts a Buck daemon process for the current project in the current working directory. When running other commands, Buck first checks for a running daemon process and if found uses that daemon to execute the command. Using a Buck daemon can save significant amounts of time as it avoids the overhead of starting a Java virtual machine, loading the buck class files and parsing the build files for each command.

The buck daemon writes its port, process id and log output to a .buckd directory it creates inside the project root directory. Subsequent buck commands use these files to find the daemon process and the buckd command uses them to kill any existing daemon process for the project when it is run.

It is safe to run several buck daemons started from different project directories and they will not interfere with each other, making buckd suitable for use in shared server environments or where several projects are being worked on concurrently.

Buck daemon processes monitor the project file system while running and invalidate cached build rules if any non source files are changed. Sub-trees within the project file system can be excluded from this monitoring by adding them as a comma separated list to the "ignore" setting in the "buckd" section of .buckconfig. Output directories should always be added to this setting to avoid cached build rules being invalidated on every run and adding .buckd, IDE directories like .idea and source control directories like .git can significantly improve performance and may be nescessary to avoid file change overflows when buck daemons are being used to build very large projects.