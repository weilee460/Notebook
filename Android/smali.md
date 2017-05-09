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
4. [Ljava/lang/String; -- 表示一个String对象数组，对象的表示都是以L开头。

 
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

### 方法调用

smali中的方法有两类：direct和virtual。direct method就是private method，public和protect都属于virtual method。调用时，分别使用invoke-direct和invoke-virtual。调用函数的还有invoke-static，invoke-super，invoke-interface等指令。

* invoke-static：调用static方法。
  
  例如：
  
  ```
  invoke-static {},Lcom/aaa;->CheckSignature()Z
  ```
  其中的{}，是调用函数的实例和参数列表，由于此函数没有参数，故为空。
  
  例如：
  
  ```
  const-string v0,"NDKLIB"
  invoke-static {v0},Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V
  ```
  v0就是参数"NDKLIB"，即 ```loadLibrary("NDKLIB")```
  
* invoke-super:调用父类方法，Android中一般是调用onCreate、onDestroy等
* invoke-direct:调用private method。
  例如：
  
  ```
  invoke-direct{p0},Landroid/app/TabActivity;-><init>()V
  ```
  即调用实例的private init()。
  
* invoke-virtual：用于调用protected和public method。
  例如：
  
  ```
  sget-object v0,Lcom/ddd;->bbb:Lcom/ccc
  invoke-virtual {v0,v1},Lcom/ccc;->Message(Ljava/lang/Object;)V
  ```
  解释：
  v0 即为bbb:Lcom/ccc
  v1就是传递给Messages方法的Ljava/lang/Object参数。

* invoke-xxxx/range：当方法的参数多于5个（包括5个）时，调用方法时在后面加上“/range”，range表示范围。
  例如：
  
  ```
  invoke-direct/range{v0...v5},Lcmb/pb/ui/PBContainerActivity;->h(ILjava/lang/CharSequence;Ljava/lang/String;Landroid/content/Intent;I)Z
  ```
  解释：
  需要传递v0到v5，总计6个参数，此时大括号内的参数采用省略形式，且需连续。
  
* move-result和move-result-object，分别用于返回基本数据类型和对象。在java中，调用函数和获取返回值用一条语句就可以完成，但在smali中，则需要分开操作来完成。
	例如：

	```
	const-string v0,"feng"
	invoke-static {v0},Lcmb.pbi;->t(Ljava/lang/string;)Ljava/lang/String;
	move-result-object v2
	```
	解释：
	v2寄存器中保存了调用t方法的返回值，类型是Ljava/langString。

### 方法实例
例如：

```
.method private isRegistered()Z
	.local 2
	.prologue
	const/4 v0,0x1
	.local v0,tempFlag:Z
	if-eqz v0, :cond_0
	const/4 v1,0x1
	:goto_0
	return v1
	:cond_0
	const/4 v1,0x0
	goto :goto_0
.end method
```

即：

```
private Boolean isRegistered()
{
	Boolean tempFlag = true;
	if (tempFlag==false)
	{
		return false;
	}
	return true;
}
```


*******

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

****

### 寄存器
Dalvik的字节码中，寄存器都是32位，能够支持任何类型；64位类型用寄存器表示。有两种方式指定一个方法中有多少几个寄存器是可用的：.registers指令指定了方法中寄存器的总数；.locals指令表示方法中的非参数寄存器的数量。
Smali中所有的操作必须经过寄存器来进行：本地寄存器用v开头数字结尾的符号表示，例如v0、v1等；参数寄存器用p开头数字结尾的符号表示，如p0、p1等。注意，p0不一定是第一个参数，在非static函数中，p0代表"this"，p1表示函数的第一个参数，p2表示函数的第二个参数...，在static函数中p0是第一个参数，原因是java中的static方法中没有this参数。

### 成员变量
定义：
```
.field public/private [static] [final] varName:<类型>
```

### 寄存器操作指令
不同的成员变量有不同的指令。这些指令分为两类：获取指令，操作指令。

获取指令有：iget、sget、iget-boolean、iget-object、sget-object等。

操作指令有：iput、sput、iput-boolean、sput-boolean、iput-object、sput-object等。

上述指令中，不带"-object"的表示操作的成员变量是基本的数据类型；带"-object"表示操作的成员变量是对象类型；特别，boolean类型使用的是带"-boolean"的指令操作

### 寄存器操作
**get指令**

例子1：

```
const/4 v0, 0x1
iput-boolean v0,p0,Lcom/aaa;->IsRegistered:Z
```
首先将值0x1存放到v0中。
使用iput-boolean指令将v0的值存放到com.aaa.IsRegistered这个变量中。
等价于(p0表示this)：

```
this.IsRegistered = true
```

例子2:  
```
sget-object vo,Lcom/aaa;->ID:Ljava/lang/String;
```  
意思是：获取类型为String的com.aaa.ID的值，并保存到本地寄存器v0中。

例子3: 
 
```
iget-object v0,p0,Lcom/aaa;->view:Lcom/aaa/view;
```
即：将类型为com.aaa.view的com.aaa.view的值保存到本地寄存器v0中，此处的p0即“this”参数。

**注意:** 获取array的值，使用aget和aget-object。

*****************
**put指令**

例子4:  

```
const/4 v3, 0x0
sput-object v3,Lcom/aaa;->timer:Lcom/aaa/timer;
```
即：```this.timer = null;```

例子5:

```
.local v0,args:Landroid/os/Message;
const/4 v1,0x12
iput v1,v0,Landroid/os/Message;->what:I
```
即```args.what = 18;```其中args的类型是android.os.Message


### 方法传参
* 当一个方法被调用的时候，方法的参数被置于最后N个寄存器中。若方法有两个参数，5个寄存器(v0-v4)，那么参数将置于最后的2个参数，即v3,v4中。  
* 非静态方法中的第一个参数总是调用该方法的对象。例如：非静态方法LMyobject;->callMe(II)有两个整型参数，此外还有一个隐含的LMyobject;的参数

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

函数调用指令：

* invoke-super 调用父类函数
* invoke-direct 调用函数
* invoke-static 调用静态函数

## Smali包信息
* .class public Lcom/aaaa;    //com.aaaa这个package下的一个类  
* .super Lcom/bbbb;   //这个类继承自com.bbbb这个类
* .source "cccc.java"  //这个smali文件由cccc.java源文件编译得到




## 参考
1.[吾爱破解安卓逆向入门教程（二）---初识APK、Dalvik字节码以及Smali](http://www.52pojie.cn/forum.php?mod=viewthread&tid=395689)  
2.[smali](https://github.com/JesusFreke/smali)  
3. [关于SMALI语法](http://bbs.pediy.com/thread-151769.htm)  
4. [吾爱破解安卓逆向入门教程（三）---深入Smali文件 ](http://www.52pojie.cn/thread-396966-1-1.html) 