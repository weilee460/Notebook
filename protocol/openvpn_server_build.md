# openVPN服务器搭建

## 环境配置
Client：Windows 10

Server: Ubuntu 14.04

## 配置方式
openVPN static key方式

## 搭建过程
1、server端安装openvpn：```sudo apt-get install openvpn```;   
2、Client端安装openvpn; 
3、生成static.key：

```
openvpn --genkey --secret static.key
``` 
3、Server配置如下：

```
dev tun
ifconfig 10.8.0.1 10.8.0.2
secret static.key
```
4、Client配置如下：

```
remote xxxxx
dev tun
ifconfig 10.8.0.2 10.8.0.1
secret static.key
```
**注：**  

	1、 xxxxxx---server的IP或者域名。

## 配置说明
secret
## 参考
1、[openvpn Windows client下载地址](https://swupdate.openvpn.org/community/releases/openvpn-install-2.4.4-I601.exe)

2、[openvpn mini配置](https://openvpn.net/index.php/open-source/documentation/miscellaneous/78-static-key-mini-howto.html)

