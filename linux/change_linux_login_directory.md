# 修改linux登陆目录

## 登陆目录
在linux系统中，远程连接登陆后，默认登陆的路径是:```/home/username```。登陆目录在```/etc/passwd```文件中定义。

## 修改
假定修改后登陆的目录是：```/samba/username```。

编辑```/etc/passwd```文件，找到对应用户名那一行。
类似于：

```
root:x:0:0:root:/root:/bin/bash
```
注意：其中的root代表用户名，此处需要找到自己的用户名的那一行。

将中间的"/root"改为想要登陆的路径即可，例如:

```
root:x:0:0:root:/samba/username:/bin/bash
```

下次登陆即可生效。

## 注意
执行上述操作时，需要root权限。否则无法编辑```/etc/passwd```文件。