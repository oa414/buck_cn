快速入门
====

**注意： Buck只能在Mac OS X和Linux上工作，不支持Windows**

这是一个Buck的快速指南,这篇文章很简洁，而不是侧重于你需要用Buck构建Android应用的命令细节。你可以在开发过程中对于针对性的主题阅读更详细的文章。

如果你构建过Android应用，你可能已经安装了Buck需要的所有工具。更多Buck安装的细节请看Downloading and Installing Buck

**注意**：本指南的命令是设计为可以复制粘贴和idempotent，同样适用于Mac OS和Linux的，有时候不同的工具（如用echo取代vi或者emaocs来建立一个文件）会有不通的结果。不过这是一个快速指南，所以更小的东西比复制粘贴更快！

安装Buck
--------




运行下面的命令从Github检出Buck，构建它，并且加入到你的$PATH里面，

.. code-block:: sh
     
      git clone git@github.com:facebook/buck.git
      cd buck
      ant
      sudo ln -s ${PWD}/bin/buck /usr/bin/buck

现在还没有下载Buck二进制包的方法。

现在，你已经在你的机器上安装了Buck了，让我们用它来编译一个简单的Android应用吧～我们应该在一个空目录下开始我们的项目，所以建一个空目录并跳转到那个目录吧。


:: 
     
      mkdir -p ~/my-first-buck-project/
      cd ~/my-first-buck-project/

注意：下面的指南会假设所有的命令都运行在 ~/my-first-buck-project 目录。

编译Java
--------

Android应用通常是用Java写的，所以首先我们设置Buck来面向Android API来编译Java。Buck需要知道你的Android SDK在哪里。假设你的Android安装在~/android-macosx-sdk，运行下面的命令来建立一个local.properties来告诉Buck从哪里找Android SDK。

::     

   echo -n sdk.dir= > local.properties && \
   echo ~/android-macosx-sdk >> local.properties

现在Buck可以找到你的Andorid SDK了，是编译Java代码的时候了。首先，我们在java/com/example/activity/MyFirstActivity.java:下建立一个简单的Activity。


::
      
      mkdir -p java/com/example/activity/
      echo "package com.example.activity;

      import android.app.Activity;
      import android.os.Bundle;

      public class MyFirstActivity extends Activity {
        
        @Override
        public void onCreate(Bundle savedInstanceState) {

        }
      }" > java/com/example/activity/MyFirstActivity.java


现在我们需要一个构建文件来定义编译Java代码的构建规则，所以我们在java/com/example/activity/BUCK里简历android_library()规则。

::

  echo "android_library(
    name = 'activity',
    srcs = glob(['*.java']),
    visibility = [ 'PUBLIC' ],
  )

  project_config(
    src_target = ':activity',
  )" > java/com/example/activity/BUCK

现在我们可以用Buck编译Java代码了。

::
   
    buck build //java/com/example/activity:activity

注意：Buck在buck-out目录产生它的输出文件，所以现在应该在你的版本控制系统里忽略到buck-out文件。

包资源
-------

Android应用通常包含资源文件，比如字符串和图片。举个例子，我们来新建只包含一个字符串的Android资源束。

::
     
      mkdir -p res/com/example/activity/res/values/
      echo "<?xml version='1.0' encoding='utf-8' ?>
      <resources>
        <string name='app_name'>Hello World</string>
      </resources>" > res/com/example/activity/res/values/strings.xml


Buck需要一个应用这些资源的途经，所以我们需要建立一个构建文件并且定义 android_resource()规则。

::

      echo "android_resource(
        name = 'res',
        res = 'res',
        package = 'com.example',
        visibility = [
          '//apps/myapp:',
        ],
      )

      project_config(
        src_target = ':res',
      )" > res/com/example/activity/BUCK

构建一个密钥
----------

实际上，你需要在物理设备上测试你的Android 应用，所以应用需要被签名。

我们会建立一个应用特定的信息，比如密钥和manifest，为了保持整洁所以在他们自己的目录下。

为了保持简单，我们会为调试创建一个自签名的证书。不幸的是，这不是一行命令就能搞定的，因为keytool命令有一堆提示。(译者注：如果Keytool提示乱码的话，简体中文用户请在此步把Console的语系改成GB2312，繁体中文用户改成Big5试试)

::
   
    keytool -genkey -keystore apps/myapp/debug.keystore -alias my_alias \
          -keyalg RSA -keysize 2048 -validity 10000

当提示了keysotre的密码，就用android把（然后打2次确认），然后按回车确认默认的名字，组织这些。

然后建立一个.properties文件存储这些信息。

::
  
    echo "key.store=debug.keystore
    key.alias=my_alias
    key.store.password=android
    key.alias.password=android" > apps/myapp/debug.keystore.properties

构建一个APK
---------

一个Android应用需要一个manifes文件，所以我们创建它：

::

  echo "<?xml version='1.0' encoding='utf-8'?>
  <manifest xmlns:android='http://schemas.android.com/apk/res/android'
            package='com.example'
            >

    <application
        android:label='@string/app_name'
        android:hardwareAccelerated='true'>
      <activity android:name='.activity.MyFirstActivity'>
        <intent-filter>
          <action android:name='android.intent.action.MAIN' />
          <category android:name='android.intent.category.LAUNCHER' />
        </intent-filter>
      </activity>
    </application>

  </manifest>" > apps/myapp/AndroidManifest.xml

现在我们在我们的构建文件里定义一个android_binary()规则。

::
    
    echo "android_binary(
      name = 'app',
      manifest = 'AndroidManifest.xml',
      target = 'Google Inc.:Google APIs:16',
      keystore_properties = 'debug.keystore.properties',
      deps = [
        '//java/com/example/activity:activity',
        '//res/com/example/activity:res',
      ],
    )

    project_config(
      src_target = ':app',
    )" > apps/myapp/BUCK

用android_library()规则构建可以产生一个APK文件

::
   
   s buck build //apps/myapp:app


如果你有一个连接到电脑的Android设备，你可以一步构建并且安装APK

创建别名
---------

每次都打buck build //apps/myapp:app很麻烦，幸运的是，Buck可以为构建目标定一个别名，用Buck命令行的时候总是可以用别名取代构建的目标。

别名必须定义在项目根目录下的config文件里面。

::
     
      echo "[alias]
          app = //apps/myapp:app" > .buckconfig

当别名就绪之后，命令行下构建和安装就简单多了

::
     
      buck install app

创建一个Intellij项目
--------------

你可能喜欢用IDE开发你的Android应用的是，Buck可以产生一个Intellij项目文件，用你在你的构建文件里面定义的 project_config() 规则

为了让Intellij组织你的Java目录，你需要在你的.buckconfig文件里面定义下面的内容

::
      
      echo "[java]
          src_roots = /java/" >> .buckconfig

现在你可以用下面的命令产生一个Intellij项目了。

注意，你可能希望在你的版本控制系统里面排除产生的文件，所以在你的.gitignore（或者.hgignore，如果你用水银的话）加入下面的内容

::    


      echo "*.iml
      /.idea/compiler.xml
      /.idea/libraries/*.xml
      /.idea/modules.xml
      /.idea/runConfigurations/Debug_Buck_test.xml" > .gitignore

现在你可以在命令行或者IDE里面构建你的Android应用啦

