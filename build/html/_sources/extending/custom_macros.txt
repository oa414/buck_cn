自定义宏
======


因为构建文件是何法的Python代码，所以可以定义函数来辅助构建规则，就像那些叫做宏的函数一样。

注意：虽然构建文件是合法的Python并且可以做任何事情（写文件，访问网络这些），但是这些事可能让Buck出一些意外的问题，所以不鼓励这样做。

比如，定义一个叫做java_library_using_guava的宏来构建一个依赖Guava的java_library构建规则.



::

def java_library_using_guava(
    name,
    srcs=[],
    resources=[],
    deps=[],
    visibility=[]):
  java_library(
    name = name,
    srcs = srcs,
    resources = resources,
    deps = [
      # This assumes this is where Guava is in your project.
      '//third_party/java/guava:guava',
    ] + deps,
    visibility = visibility,
  )



调用它就像内建的规则一样


::

  # Calling this function has the side-effect of creating
  # a java_library() rule named 'util' that depends on Guava.
  java_library_using_guava(
    name = 'util',
    # Source code that depends on Guava.
    srcs = glob(['*.java']),
  )


你可以构建一些更复杂的宏，比如，你可能想创建一个能同时产生一个调试用和发布用的apk文件。

:: 

    def create_apks(
        name,
        manifest,
        debug_keystore_properties,
        release_keystore_properties,
        proguard_config,
        deps):

      # This loop will create two android_binary rules.
      for type in [ 'debug', 'release' ]:
        # Select the appropriate keystore.
        if type == 'debug':
          keystore_properties = debug_keystore_properties
        else:
          keystore_properties = release_keystore_properties

        android_binary(
          # Note how we must parameterize the name of the
          # build rule so that we avoid creating two build
          # rules with the same name.
          name = '%s_%s' % (name, type),
          manifest = manifest,
          target = 'Google Inc.:Google APIs:16',
          keystore_properties = keystore_properties,
          package_type = type,
          proguard_config = proguard_config,
          deps = deps,
          visibility = [
            'PUBLIC',
          ],
        )
    )


使用起来就像是内建的构建规则一样。

::

    create_apks(
      name = 'messenger',
      manifest = 'AndroidManifest.xml',
      debug_keystore_properties = 'debug.keystore.properties',
      release_keystore_properties = 'release.keystore.properties',
      proguard_config = 'proguard.cfg',
      deps = [
        # ...
      ],
    )

注意，如果这些定义在apps/messenger/BUCK里面，这会创建以下的构建规则：


::

    //apps/messenger:messenger_debug
    //apps/messenger:messenger_release


然而，这个构建规则是不存在的

::
    //apps/messenger:messenger

这可能让期望用下面命令的人失望。

::
    buck build //apps/messenger:messenger
    buck targets --type create_apks

在你的宏前面加上公司的名字可以容易和内建的规则区分，同时，不加的话，自定义宏和内建的规则一样统一，怎么做随你便、

