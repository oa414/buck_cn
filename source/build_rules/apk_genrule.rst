一个apk_genrule()规则用来预处理一个APK，从genrule分离出apk_genrule来输出APKs。所以像buck install 或者 buck uninstall 仍然能工作

参数

名字（要求）规则的名字
apk（要求）输入的android_binary()规则。产APK的路径可以关联到$APK shell变量

srcs(要求) 等同与genrule().
deps(要求) 等同与genrule().
cmd(要求) 等同与genrule().
cout(要求) 等同与genrule().
- visibility (默认是[])， 构建目标模式的列表，用来定义这个规则是否能被包含在其他构建规则里面。

例子

这里有一个一对产生APK的apk_rule()，做了一些签名，以及zipalign(字节对齐)的工作


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

apk_genrule(
  name = 'messenger_super_sign_unalign',
  apk = ':messenger',
  deps = [
    '//java/com/facebook/sign:super_sign',
  ],
  cmd = '${//java/com/facebook/sign:super_sign} --input $APK'
      '--output $OUT',
  out = 'messenger_super_sign_unalign.apk',
)

apk_genrule(
  name = 'messenger_super_sign',
  apk = ':messenger_super_sign_unalign',
  deps = [],
  cmd = '$ANDROID_HOME/tools/zipalign -f 4 $APK $OUT',
  out = 'messenger_super_sign.apk',
)
