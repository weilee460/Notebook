# Android develop environment config
Android Studio安装配置，以windows为例

## Android Studio
### Install
**前置条件： 系统已经安装java，并配置好** 
 
1. 下载Android Studio,[Android Studio](https://developer.android.google.cn/studio/index.html)。国内已经可以正常访问。
2. 将下载下来的Android Studio安装好。一路点击下去即可，注意安装的路径。
3. 安装好之后，打开Android Studio，打开SDK Manager，安装好需要的SDK以及虚拟机镜像等。

### Config

## 环境变量配置
此处Android sdk安装路径是:"D:\Android\sdk"

1. 设置adb命令路径。在环境变量path中添加  
```
D:\Android\sdk\platform-tools
```
2. 设置avdmanager命令路径。在path中添加  
```
D:\Android\sdk\tools\bin
```
3. 设置ndk命令路径，在path中添加  
```
D:\Android\sdk\ndk-bundle
```
4. 设置`emulator`命令的路径  
`
D:\Android\sdk\emulator
`
5.  


## Emulator
`emulator`命令由两个，分别在sdk目录下的tools文件夹和emulator文件夹下，而tools目录下的emulator不能用来从cmd中启动emulator，必须使用emulator目录下的emulator。



## 参考
1. [Android Studio](https://developer.android.google.cn/studio/index.html)
2. 
