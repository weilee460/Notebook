# Smali

## 基本语法
### 类型
Dalvik字节码有两种类型：原始类型，引用类型。
#### 原始类型
* B -- byte
* C -- char
* D -- double(64位)
* F -- float
* I -- int
* J -- long(64位)
* S -- short
* V -- void
* Z -- boolean
* [XXX -- array
* Lxxx/yyy -- object

**注：**
1. 对象以Lpackage/name/ObjectName; 的形式表示，前面的"L"表示这是一个对象类型，package/name/是该对象所在的包，ObjectName是对象的名字，";"表示对象名称结束。相当于java中的package.name.ObjectName。例如：Ljava/lang/String;相当于java/lang.String；  
2. [I -- 表示一个整型的一维数组，相当于java的"int[]"；  
3. [[I -- 相当于"int[][]"，类似的"[[[I"相当于"int[][][]"；  
4. [Ljava/lang/String; -- 表示一个String对象数组。

 
***

### 方法
一般定义：Funcname(Para-type1Para-type2...)Return-type  

**注：**  
1. hello()V  --- void hello()  
2. hello(III)Z --- boolean hello(int, int, int)  
3. hello(Z[I[ILjava/lang/String;J)Ljava/lang/String --- String hello(boolean, int[], int[], String, long)

表示形式：Lpakage/name/ObjectName;->MethodName(III)Z  
**注：**
1. Lpakage/name/ObjectName;表示类型；MethodName是方法名；III表示参数，此处是3个整型参数；Z是返回类型，此处是boolean。  
2. 方法的参数是一个接一个，中间没有隔开，没有空格。  

***

### 基本语法
* field private isFlag:z  定义变量
* .method 方法
* .parameter 方法参数
* .prologue 方法开始
* .line 123  此方法位于第123行
* invoke-super 调用父类函数
* const/high16 v0, 0x7fo3  把0xfo3赋值给v0
* invoke-direct 调用函数
* return-void 函数返回void
* .end method 函数结束
* new-instance 创建实例
* input-object  对象赋值
* iget-object 调用对象
* invoke-static 调用静态函数


## 基本程序结构
### 条件判断

1. "if-eq Va, Vb,:cond_xx"   若Va等于Vb则跳转到cond_xx；
2. "if-ne Va, Vb,:cond_xx"   若Va不等于Vb则跳转到cond_xx；
3. "if-lt Va, Vb,:cond_xx"   若Va小于Vb则跳转到cond_xx；
4. "if-ge Va, Vb,:cond_xx"   若Va大于等于Vb则跳转到cond_xx；
5. "if-gt Va, Vb,:cond_xx"   若Va大于Vb则跳转到cond_xx；
6. "if-le Va, Vb,:cond_xx"   若Va小于等于Vb则跳转到cond_xx；
7. "if-eqz Va,:cond_xx"   若Va等于0则跳转到cond_xx；
8. "if-nez Va,:cond_xx"   若Va不等于0则跳转到cond_xx；
9. "if-ltz Va,:cond_xx"   若Va小于0则跳转到cond_xx；
10. "if-gtz Va,:cond_xx"   若Va大于0则跳转到cond_xx；
11. "if-gez Va,:cond_xx"   若Va大于等于0则跳转到cond_xx；
12. "if-lez Va,:cond_xx"   若Va小于等于0则跳转到cond_xx；

## 函数调用

## Smali



## 参考
1.[吾爱破解安卓逆向入门教程（二）---初识APK、Dalvik字节码以及Smali](http://www.52pojie.cn/forum.php?mod=viewthread&tid=395689)  
2.[smali](https://github.com/JesusFreke/smali)  
3. [关于SMALI语法](http://bbs.pediy.com/thread-151769.htm)