# Android逆向

##APK文件结构
将apk文件的后缀名由“apk”改为"rar"，使用解压软件打开。目录如下：

```
---assets
---com
---lib
---META-INF
---res
---AndroidManifest.xml
---classes.dex
---resources.arsc
```
说明：
  
* assets目录存放一些配置文件等。
* lib目录存放了一些公用的库，含有以CPU架构命名的子目录。
* res目录存放资源文件，例如图片，视频等。
* META-INF目录下存放的是签名信息。
* classes.dex是java编译后生成的java字节码文件。
* resources.arsc是编译后的二进制资源文件。
* AndroidManifest.xml是应用的描述文件，含有应用名、版本等信息。apk中的文件是压缩过的，可以使用AXMLPrinter2工具解开。命令：
```
java -jar AXMLPrinter2.jar AndroidManifest.xml
```

##反编译
反编译：
下载apktool工具：[apktool](https://bitbucket.org/iBotPeaches/apktool/downloads/)

```
java -jar apktool_2.2.2.jar d xxx.apk

注："xxx.apk" --- apk文件名，修改为自己需要反编译的
```

##修改文件
此处假设修改的是AndroidMainifest.xml，添加调试属性
```
android:debuggable="true"
```

```
    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme"
        android:debuggable="true" >
    </application>
```

##打包
打包命令：

```
java -jar apktool_2.2.2.jar b xxx

注："xxx"是反编译apk后的文件夹；生成的apk文件在xxx目录的dist目录下。
```


##签名

###生成keystore
```
keytool -genkey -v keystore mykeystore(改为自己想要的名字) -alias mingming(任意名，后续的签名要用) -keyalg RSA -validity 20000
```
然后根据提示输入密码等。

###签名apk
```
jarsigner -verbose -keystore mykeystore -signedjar xxx_signed.apk(签名后生成的apk名字) xxx.apk mingming(生成keystore时的别名)
```


##参考
1. [apktool下载](https://bitbucket.org/iBotPeaches/apktool/downloads/)  
2. [apktool](https://ibotpeaches.github.io/Apktool/)
3. [oracle jar signing](http://docs.oracle.com/javase/tutorial/deployment/jar/signing.html)

