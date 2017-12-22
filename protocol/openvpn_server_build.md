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
4、Server配置如下：

```
dev tun
ifconfig 10.8.0.1 10.8.0.2
secret static.key
push "redirect-gateway def1 bypass-dhcp"   //允许客户端重定向穿越VPN到外网
push "dhcp-option DNS 8.8.8.8"    //配置服务端的DNS
push "dhcp-option DNS 8.8.4.4"
```

5、Server路透设置

```
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -j SNAT --to-source server公网IP
```

6、Client配置如下：

```
remote xxxxx
dev tun      //基于路由的隧道
ifconfig 10.8.0.2 10.8.0.1
secret static.key
redirect-gateway def1     //使客户端中所有的流量都经过vpn
dhcp-option DNS 8.8.8.8    //设置客户端VPN的DNS
dhcp-option DNS 8.8.4.4
```
**注：**  

	1、 xxxxxx---server的IP或者域名。

## 配置说明

secret

## 参考
1、[openvpn Windows client下载地址](https://swupdate.openvpn.org/community/releases/openvpn-install-2.4.4-I601.exe)

2、[openvpn mini配置](https://openvpn.net/index.php/open-source/documentation/miscellaneous/78-static-key-mini-howto.html)

