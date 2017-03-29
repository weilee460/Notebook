# Android逆向

##APK文件


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
keytool -genkey -v keystore mykeystore -alias mingming -keyalg RSA -validity 20000
```
然后根据提示输入密码等。

###签名apk
```
jarsigner -verbose -keystore mykeystore -signedjar xxx_signed.apk xxx.apk mingming
```




##参考
1. [apktool下载](https://bitbucket.org/iBotPeaches/apktool/downloads/)  
2. [apktool](https://ibotpeaches.github.io/Apktool/)

