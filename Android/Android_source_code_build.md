# Android 源码编译

## 系统
1. macOS Sierra.
2. Android.7.1.1
3. Mac OSX SDK 10.11

## 编译环境

1. 安装jdk，设置环境变量。

	```
	export ANDROID_JAVA_HOME=$(/usr/libexec/java_home -v 1.8)
	```
	
2. 安装Xcode，以及命令行工具.
3. 安装MacPort，需科学上网。[MacPort官网](https://www.macports.org/install.php)。设置环境变量：

	```
	export PATH=/opt/local/bin:$PATH
	```
4.  安装编译依赖包：

	```
	POSIXLY_CORRECT=1 sudo port install gmake libsdl git gnupg
	```

5. 设置同时打开文件数目：

	```
	ulimit -S -n 1024
	```
	
6. 下载sdk:[MacOSX SDK](https://github.com/phracker/MacOSX-SDKs).拷贝到```/Applications/XCode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs```

## 下载源码

1. 建立区分大小写的磁盘镜像，命令如下：

	```
 	hdiutil create -type SPARSE -fs 'Case-sensitive Journaled HFS+' -size 140g ~/android.dmg
	```
2. 挂载镜像：
	```
	function mountAndroid { hdiutil attach ~/android.dmg -mountpoint /Volumes/android; }
	```
3. 建立工作目录：

	```
	mkdir aosp
	```
4. 进入工作目录，下载版本工具repo，需科学上网：

	```
	curl https://storage.googleapis.com/git-repo-downloads/repo > repo
	chmod u+x repo
	```
5. 配置git信息：

	```
	git config --global user.name "Your Name"
	git config --global user.email "you@example.com"
	```
6. 初始化工作目录，需科学上网：

	```
	repo init -u https://android.googlesource.com/platform/manifest
	```
	指定版本时：
	
	```
	repo init -u https://android.googlesource.com/platform/manifest -b android-7.1.1_r1
	```
7. 下载源码，需科学上网：

	```
	repo sync
	```

注意：也可以使用清华大学的镜像。

## 编译源码

1. 修改编译配置文件，路径："build/core/combo/mac_version.mk"

	```
	mac_sdk_versions_supported := 10.8 10.9 10.10 10.11
	
	修改为
	mac_sdk_versions_supported := 10.8 10.9 10.10 10.11
	```

2. 清理编译环境：

	```
	make clobber
	```

3. 建立编译环境：

	```
	source build/envsetup.sh
	```

4. 设立编译目标：

	```
	lunch aosp_arm-eng
	```
5. 编译：

	```
	make -j4
	```
	
6. 启动虚拟机：

	```
	emulator
	```

## 错误排查


## 参考
1. [Android_Goole](https://source.android.com/source/initializing)

2. [清华大学镜像站点](https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/#section-1)

3. [macOS（Sierra 10.12）上Android源码（AOSP）的下载、编译与导入到Android Studio
](http://blog.bihe0832.com/macOS-AOSP.html?utm_source=tuicool&utm_medium=referral)

4. [解决macOSX10.12.SDK下编译Android Open Source Project出错的问题](http://palanceli.com/2016/09/25/2016/0925AOSPOnMac/)

