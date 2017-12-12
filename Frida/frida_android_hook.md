# Frida Android App Hook

##Frida简介
官网的简介是
> Dynamic instrumentation toolkit for developers, reverse-engineers, and security researchers.

简而言之：Frida是个动态注入工具。适用于Windows/Linux/iOS/Android平台。


##Frida Android 环境设置
1. 安装python。建议安装最新版本的python。
2. 安装frida（需要root权限）：```pip install frida```
3. 下载frida server。选择适合自己手机的版本。
4. 将frida server复制到手机上，并赋予可执行权限（手机需root）。
5. 启动frida server：```./frida-server```
6. 配置Android端口转发如下：

``` 
	adb forward tcp:27042 tcp:27042
	adb forward tcp:27043 tcp:27043
```

##验证环境设置
执行如下命令：

```
	frida-ps -R
```
若输出当前手机正在运行的进程，则说明环境配置正确

##Hook Android java层函数
###测试Android App主要代码
```
	pakage com.feng.lee.fridahookdemo;
 
    private boolean checkUsername(String username)
    {
        if (username.equals("ying")) return true;
        return false;
    }
 
    private boolean checkPwd(int pwd)
    {
        if (pwd == 123456) return true;
        return false;
    }
 
    private boolean checkUsernameAndPwd(String username, String pwd)
    {
        if (username.equals("China") && pwd.equals("123456")) return true;
        return false;
    }
```
注：这是一个验证用户输入用户名和密码的简易App，只要用户名为“China”，密码为“123456”，则弹出正确的Toast，否则弹出出现错误的Toast。

###Frida python Scripts
```
# -*- coding: utf-8 -*-
import frida
import sys
import time
 
rdev = frida.get_remote_device()
process = rdev.attach("com.feng.lee.fridahookdemo")
 
def on_message(message, data):
    if message['type'] == 'send':
        print("[*] {0}".format(message['payload']))
    elif message['type'] == 'error':
        print(message['stack'])
    else:
        print(message)
 
jscode = """
    Java.perform(function() {
        send("Running Scripts");
        var MainActivity = Java.use("com.feng.lee.fridahookdemo.MainActivity");
        
        send(Object.getOwnPropertyNames(MainActivity));
        
        MainActivity.onResume.implementation = function() {
            send("onResume");
            this.onResume();
        };
        
        MainActivity.checkUsername.implementation = function(v1) {
            send("checkUsername: username = "+JSON.stringify(v1));
            return this.checkUsername(v1);
        };
        
        MainActivity.checkPwd.implementation = function(v2) {
            send("checkPwd: pwd = "+JSON.stringify(v2));
            return this.checkPwd(v2);
        };
 
        MainActivity.checkUsernameAndPwd.implementation = function(v1, v2) {
            send("checkUsernameAndPwd name = "+ JSON.stringify(v1) + " " + JSON.stringify(v2));
            return true;
        }; 
        
    })    
"""
script = process.create_script(jscode)
script.on('message',on_message)
script.load()
sys.stdin.read()
```

###Hook输出



##参考
1、[Frida官网](https://www.frida.re/)；

2、[Frida server下载地址](https://github.com/frida/frida/releases)；
