# sshfs挂载samba

## 缘起

由于常用的开发环境是Linux Server + client，其中client为Windows，Mac。项目文件通过samba共享。使用Mac开发时，通过Mac自带的smb连接samba服务器，在使用Eclipse打开项目后，点击项目内的函数，出现问题：同文件内的函数可以顺利的跳转，而不同文件内的函数不可以顺利的跳转，导致在Mac端编码比较麻烦。

使用google搜索后，无意发现一个不同的mount方式。尝试了一下，配置好后，使用Eclipse打开项目，惊讶的发现，居然可以正确的跳转到不同文件的函数。

## 配置

1. 安装homebrew；
2. 安装homebrew cask；
3. 安装osxfuse：  ```brew cask install osxfuse```
4. 安装sshfs： ```brew install sshfs```


## 使用

1. mount服务器文件夹： ```sshfs username@server:path  local_path```
2. umount：  ```umount local_path```


## 参考
1. [homebew](https://brew.sh/)

2. [homebrew cask](https://caskroom.github.io)

3. [sshfs](https://github.com/osxfuse/osxfuse/wiki/SSHFS)

4. [sshfs_github](https://github.com/libfuse/sshfs)