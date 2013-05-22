An android_binary() rule is used to generate an Android APK.

一个android_binary()规则用来生成一个Android APK.

参数
----

- name（必须）规则的名字，也是这个规则产生的apk的名字。
- manifest(必须) Andorid manifest的相对路径，通常情况下会和android_binary()在同一个目录，在这种情况下可以简单写成‘AndroidManifest.xml’
- target(必须)构建APK的目标Android版本。在传统Android 项目中，通常在project.properties文件定义，通常是类似Google Inc.:Google APIs:16
- keystore_properties(必须) 定义keystore属性的.properties文件的相对路径。


key.store=debug.keystore
key.alias=my_alias
key.store.password=android
key.alias.password=android

注意key.store的值必须是keystore自己的相对路径。

更多细节请看Quick_Start.

- package_type(默认是debug), 决定是否在打包apk的时候用ProGuard.package_type允许的值是'debug'和'release'.默认的值是debug，指定ProGuard不运行
- 
注意 这个参数将会被重命名来直观地反映是用来确定运行ProGuard的

- proguard_config 默认是None，  ProGurad的配置文件的相对路径。通过-include标记传递，当package_typ是release的时候。
-  no_dx 默认是[]，通过编译的时候传递的android_library和java_library()依赖的构建目标的列表。所以不应该包含在生成的
- apk里的classes.dex
- deps 默认是[] 将会打包到apk文件的java代码，资源文件以及native的库。通过传递，下面的规则类型将会被打包进apk里面。
- 
android_library()
android_resource()
java_library()
java_binary()
prebuilt_jar()
ndk_library()
prebuilt_native_library()

- visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。
- 
例子

这里有一个android_binary()的规则，包含了一个Android资源文件的依赖和一个已编译的java代码的依赖。



android_resource(
  name = 'res',
  res = 'res',
  assets = 'assets',
)

android_library(
  name = 'src',
  srcs = glob(['src/**/*.java']),
  deps = [
    ':res',
  ],
)

# Building this rule will produce a file named messenger.apk.
android_binary(
  name = 'messenger',
  manifest = 'AndroidManifest.xml',
  target = 'Google Inc.:Google APIs:16',
  keystore_properties = 'keystore.properties',
  package_type = 'release',
  proguard_config = 'proguard.cfg',
  deps = [
    ':res',
    ':src',
  ],
)