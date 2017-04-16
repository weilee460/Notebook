# Android 源码编译

## 编译平台
以下针对Mac 平台

## 创建大小写敏感的分区
由于系统在重新安装格式化磁盘时，选择的是区分大小写敏感的格式，因此，此处未坐尝试。

## 建立编译环境

### 安装Java
不同的Android版本需要不同版本的JDK，根据需要编译的Android版本安装对应的版本的JDK即可。见 参考1

### 安装Xcode command line tools
```
xcode-select --install
```

### 安装MacPorts
从MacPorts官网下载软件包安装即可。注意：安装时会出现安装界面卡住，安装不能完成的现象，请科学上网再次安装即可。








## 参考
1. [Android](https://source.android.com/source/requirements)
2. [MacPorts](https://www.macports.org/install.php)