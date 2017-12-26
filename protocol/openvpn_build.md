# openvpn编译

## openvpn-gui编译

### 编译目的

在Windows平台上编译openvpn，获得openvpn-gui应用。

### 编译环境
Windows，Cygwin

### 编译方法

官方指导手册，即[openvpn-gui build](https://github.com/OpenVPN/openvpn-gui/blob/master/BUILD.rst)。  

### 编译步骤 
1、安装Cygwin。  
2、安装依赖的软件包：
> autoconf  automake  pkg-config  make  mingw64-x86_64-gcc-core   
> mingw64-x86_64-g++  mingw64-x86_64-openssl

3、进入cywin命令行界面，执行编译命令：

```
autoconf -iv
./configure --host=x86_64-w64-mingw32
make
```


## openvpn编译


## 参考
1、[openvpn-gui build](https://github.com/OpenVPN/openvpn-gui/blob/master/BUILD.rst)  
2、[Cygwin](https://www.cygwin.com/)

