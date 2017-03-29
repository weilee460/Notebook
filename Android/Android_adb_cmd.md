# Android adb命令

## 常见命令

1. 显示系统中全部的android平台

```
avdmanager list targets
```
2. 显示系统中全部的avd 
 
```
avdmanager list avd
```
3. 显示当前运行全部设备
```
adb devices
```
4. 对某一模拟器执行命令

```
adb -s 模拟器编号 命令
```
5.