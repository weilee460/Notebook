# Xposed develop

## Xposed原理
Android设备启动部分过程：有一个进程叫做“Zygote”，这个进程是Android运行时的核心。每一个应用都是fork它启动的。这个进程由/init.rc脚本，在手机启动时启动起来的。这个进程执行完成以后，启动/system/bin/app_process，这个进程加载所需的类，触发初始化方法。  
Xposed就是将上述的/system/bin/app_process替换掉，使用"extended app_process"，这个"extended app_process"添加了额外的jar到classpath中，从特定的路径调用方法。

官方描述如下：
>Before beginning with your modification, you should get a rough idea how Xposed works (you might skip this section though if you feel too bored). Here is how:

>There is a process that is called "Zygote". This is the heart of the Android runtime. Every application is started as a copy ("fork") of it. This process is started by an /init.rc script when the phone is booted. The process start is done with /system/bin/app_process, which loads the needed classes and invokes the initialization methods.

> This is where Xposed comes into play. When you install the framework, an extended app_process executable is copied to /system/bin. This extended startup process adds an additional jar to the classpath and calls methods from there at certain places. For instance, just after the VM has been created, even before the main method of Zygote has been called. And inside that method, we are part of Zygote and can act in its context.

> The jar is located at /data/data/de.robv.android.xposed.installer/bin/XposedBridge.jar and its source code can be found here. Looking at the class XposedBridge, you can see the main method. This is what I wrote about above, this gets called in the very beginning of the process. Some initializations are done there and also the modules are loaded (I will come back to module loading later).


## Xposed模块开发
使用Android Studio建立一个空Activity的Android App项目。下载Xposed API。  
API 下载：[Xposed API](https://bintray.com/rovo89/de.robv.android.xposed/api)
链接中有两个下载，分别是：api-82.jar和api-82-sources.jar，其中api-82.jar就是模块用的api，api-82-sources.jar包含有文档，如果不用看，可以不用下载下来。

### 加入API库
在app目录下建立lib文件夹，加入下载得到的api-82.jar。

### 引入Xposed API
在上面建立的项目中：app/build.gradle文件中增加：

```
repositories {
    jcenter();
}

dependencies {
    provided 'de.robv.android.xposed:api:82'
    provided 'de.robv.android.xposed:api:82:sources'
}
```

其中 
```
provided 'de.robv.android.xposed:api:82:sources'
```
包含了文档，若不用，可以不用添加。

### 声明Xposed模块

在项目的AndroidManifest.xml文件中添加如下代码： 
 
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="de.robv.android.xposed.mods.tutorial"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk android:minSdkVersion="18" />

    <application
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name" >
        <meta-data
            android:name="xposedmodule"
            android:value="true" />
        <meta-data
            android:name="xposeddescription"
            android:value="Easy example which makes the status bar clock red and adds a smiley" />
        <meta-data
            android:name="xposedminversion"
            android:value="53" />
    </application>
</manifest>
```
说明：
1.`<meta-data android:name="xposedmodule" android:value="true" />`表明这个是Xposed模块。  
2.`<meta-data android:name="xposedminversion" android:value="53" />`表明支持的最低版本。

 
### 模块实现

在src/main/java/包名，新建一个类，假设类名是“Main”，需要hook方法是是"checkLogin(String str)", 则实现如下：

```
package com.feng.lee.hookdemo;

import de.robv.android.xposed.IXposedHookLoadPackage;
import de.robv.android.xposed.XC_MethodHook;
import de.robv.android.xposed.XposedBridge;
import de.robv.android.xposed.XposedHelpers;
import de.robv.android.xposed.callbacks.XC_LoadPackage;

import static de.robv.android.xposed.XposedHelpers.findAndHookMethod;
import static de.robv.android.xposed.XposedHelpers.findField;

public class Main implements IXposedHookLoadPackage {
        @Override
        public void handleLoadPackage(XC_LoadPackage.LoadPackageParam loadPackageParam) throws Throwable {
            if (loadPackageParam.packageName.equals("com.feng.lee.testhookdemo")) {
                XposedBridge.log("Xposed loaded app: " + loadPackageParam.packageName);

                findAndHookMethod("com.feng.lee.testhookdemo.MainActivity", loadPackageParam.classLoader,
                        "checkLogin", String.class, new XC_MethodHook(){
                            @Override
                            protected void beforeHookedMethod(MethodHookParam param) throws Throwable {
                                //super.beforeHookedMethod(param);
                                XposedBridge.log("Before Hooking---param.args[0]" + param.args[0]);
                            }

                            @Override
                            protected void afterHookedMethod(MethodHookParam param) throws Throwable {
                                //super.afterHookedMethod(param);
                                XposedBridge.log("After Hooking---param.args[0]" + param.args[0]);
                            }
                        });

            }
            else {
                XposedBridge.log("Xposed not loaded app: " + loadPackageParam.packageName);
            }
        }
```

### 声明模块入口
在app/main/src/main下，新建assets目录，在目录内新建xposed_init文件。
在文件中添加如下代码：

```
com.feng.lee.hookdemo.Main
```

说明：
`com.feng.lee.hookdemo.Main`中Main是类名，前面是包名。

### 编译安装
不要直接使用Android Studio中的run，是不能直接运行的。使用build中的"Generate Signed APK"选项编译出一个签名的apk文件，然后安装到手机上。安装好以后，打开Xposed中的模块，勾选上这个模块，重启手机，即可生效。在Xposed的日志中即可查看到输出的信息。


## 参考
1. [Xposed模块的开发](http://www.snowdream.tech/2016/09/02/android-develop-xposed-module/)
2. [Development tutorial](https://github.com/rovo89/XposedBridge/wiki/Development-tutorial)
3. [Using the Xposed Framework API](https://github.com/rovo89/XposedBridge/wiki/Using-the-Xposed-Framework-API)
4. [Xposed API](https://bintray.com/rovo89/de.robv.android.xposed/api)
5. [Development tutorial](https://github.com/rovo89/XposedBridge/wiki/Development-tutorial)
