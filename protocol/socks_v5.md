# SOCKS Protocol Version 5

## socks5通信过程
**连接：** 首先socks5代理客户端需要与浏览器建立tcp连接，发送协商报文R1；客户端收到协商报文后，回复协商响应报文A1。
  
**请求：** 上述连接过程完成后，客户端就向服务端发送详细请求报文R2。服务端响应该请求，并发送一个响应报文A2，告诉客户端请求结果。若上述的连接过程中的协商方法有完整性检查或安全性检查的要求，则请求报文R2需要根据要求封装报文。在响应报文A2中，BND.PORT是服务器分配的端口号，BND.ADDR是服务器分配的地址，若服务器有多个地址，则A2报文中的BND.ADDR可能跟客户端连接的socks服务器地址不同。socks服务器利用R2报文中DST.ADDR和DST.PORT，以及客户端的地址和端口，对客户端的connect请求进行分析。

## 报文格式

浏览器与客户端连接报文R1：

```
   +----+----------+----------+
   |VER | NMETHODS | METHODS  |
   +----+----------+----------+
   | 1  |    1     | 1 to 255 |
   +----+----------+----------+                               
```

* 此处的ver值是5，表示版本5；
* nmethods的值表示随后的methods有多少个；
* methods表示支持的method，范围是1～255。

****

客户端响应连接报文A1：

```                   
    +----+--------+
    |VER | METHOD |
    +----+--------+
    | 1  |   1    |
    +----+--------+
```

* 此处的ver值是5，表示版本5；
* method表示在上述请求报文中的选择的一个method。若值是0xff，则请求报文中列举的方法全不支持；若值是0x00，则"no authentication required"；若值是0x01，则"GSSAPI"；若是0x02，则"username/password"; 若是0x03，则"to X'7F' IANA ASSIGNED"；若是0x80，则 "to X'FE' Peserved for private methods"; 若是0xFF，则 "No Acceptable methods".

***

访问web等代理请求报文R2：

```                   
    +----+-----+-------+------+----------+----------+
    |VER | CMD |  RSV  | ATYP | DST.ADDR | DST.PORT |
    +----+-----+-------+------+----------+----------+
    | 1  |  1  | X'00' |  1   | Variable |    2     |
    +----+-----+-------+------+----------+----------+
```
* 此处ver值为5；
* CMD表示命令类型，可能取值如下：
	* connect 0x01;
	* bind 0x02;
	* udp 0x03;
* RSV值是保留位，默认为0；
* ATYP(address type of following address)表示地址类型，可能取值如下:
	* IPv4  0x01;
	* domainname 0x03;
	* IPv6 0x04;
* DST.ADDR desired destination address;
* DST.PORT desired destination port in network octet order. 

*****

响应访问请求报文A2：

```        
        +----+-----+-------+------+----------+----------+
        |VER | REP |  RSV  | ATYP | BND.ADDR | BND.PORT |
        +----+-----+-------+------+----------+----------+
        | 1  |  1  | X'00' |  1   | Variable |    2     |
        +----+-----+-------+------+----------+----------+        
```

* 此处ver同上，值为5；
* REP(reply field):
	* 0x00  succeeded
	* 0x01  general SOCKS server failure
	* 0x02  connection not allowed by ruleset
	* 0x03  network unreachable
	* 0x04  host unreachable
	* 0x05  connection refused
	* 0x06  TTL expired
	* 0x07  command not supported
	* 0x08 address type not supported
	* 0x09 to 0xFF unassigned
* RSV 保留
* ATYP (address type of following address):
   * 0x01  IPv4 address
   * 0x03  domainname
   * 0x04  IPv6
* BND.ADDR server bound address
* BND.PORT server bound port in network octet order 

## 报文截取

### Mac
使用wireshark截取lo网卡的包即可。

### Windows
安装wireshark后，不能截取环回链路的网络报文。原因是winpcap不支持环回链路的报文截取，需要将winpcap卸载掉，安装npcap。参考npcap官网。   
**注意：** 作者未尝试过。

## 参考
1. [RFC1928](https://www.ietf.org/rfc/rfc1928.txt)
2. [科学上网之socks5代理原理](http://www.aichengxu.com/wangluo/9370686.htm)
3. [Npcap](https://nmap.org/npcap/)  
