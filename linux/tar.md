# tar命令使用

## 命令参数
参数说明:

* -c：建立一个压缩文件的参数指令(create 的意思)；  
* -x：解开一个压缩文件的参数指令！  
* -t ：查看 tarfile 里面的文件！
* 特别注意，在参数的下达中， c/x/t 仅能存在一个！不可同时存在！因为不可能同时压缩与解压缩。  
* -z ：是否同时具有 gzip 的属性？亦即是否需要用 gzip 压缩？  
* -j ：是否同时具有 bzip2 的属性？亦即是否需要用 bzip2 压缩？
* -v ：压缩的过程中显示文件！这个常用，但不建议用在背景执行过程！
* -f ：使用档名，请留意，在 f 之后要立即接档名喔！不要再加参数！
* -p ：使用原文件的原来属性（属性不会依据使用者而变）
* -P ：可以使用绝对路径来压缩！
* -N ：比后面接的日期(yyyy/mm/dd)还要新的才会被打包进新建的文件中！

## tar压缩
```
tar -cvf /tmp/etc.tar /etc <==打包，不压缩！
tar -zcvf /tmp/etc.tar.gz /etc <==打包并以 gzip 压缩
tar -jcvf /tmp/etc.tar.bz2 /etc <==打包并以 bzip2 压缩
```
## tar解压缩
```
tar -xvf /tmp/etc.tar /home <==解压缩
tar -zxvf /tmp/etc.tar.gz /home <==解压缩gzip
tar -jxvf /tmp/etc.tar.bz2 /home <==解压缩bzip2
```
## 小结
```
tar –xvf file.tar //解压 tar包 
tar -xzvf file.tar.gz //解压tar.gz 
tar -xjvf file.tar.bz2 //解压 tar.bz2 
tar –xZvf file.tar.Z //解压tar.Z 
unrar e file.rar //解压rar 
unzip file.zip //解压zip 
```
