# Dol开发环境配置

## 一、Description

Distributed Operation Layer : 
​	The distributed operation layer (DOL) is a software development framework to program parallel applications. The DOL allows to specify applications based on the Kahn process network model of computation and features a simulation engine based on SystemC. Moreover, the DOL provides an XML-based specification format to describe the implementation of a parallel application on a multi-processor systems, including binding and mapping.

![dol](/lab1/dol_structural.png)

## 二、How to install

本次实验环境在linux下进行，建议使用虚拟机安装Ubuntu

[VMWARE教程]([http://](http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html)[jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html](http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html))

[VIRTUALBOX教程]([http://](http://jingyan.baidu.com/article/cdddd41c5eea3153ca00e160.html)[jingyan.baidu.com/article/cdddd41c5eea3153ca00e160.html)

[Ubuntu下载]([http://](http://www.ubuntu.com/download/desktop)[www.ubuntu.com/download/desktop](http://www.ubuntu.com/download/desktop))

### 1.安装一些必要的环境

```
$  sudo apt-get update
$  sudo apt-get install ant
$  sudo apt-get installopenjdk-7-jdk
$  sudo apt-get install unzip
```

### 2.下载文件

```
$  sudo wget http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz
$  sudo wget http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip
```

### 3.解压文件

```
$  mkdir dol                       //新建dol的文件夹
$  unzipdol_ethz.zip -d dol        //将dolethz.zip解压到 dol文件夹中
$  tar -zxvf systemc-2.3.1.tgz     //解压systemc
```

### 4.编译systemc

```
$  cdsystemc-2.3.1                //解压后进入systemc-2.3.1的目录下
$  mkdir objdir                   //新建一个临时文件夹objdir
$  cdobjdir                       //进入该文件夹objdir
$  ../configure CXX=g++--disable-async-updates    //运行configure
```

运行后得到以下结果
![run outcome](/lab1/dol4.png)


```
$  sudo make install            //编译
```
编译完后文件目录如下
```
$ cd ..        $ ls
```
![content](/lab1/dol1.png)
```
记录当前的工作路径
$  pwd                          //输出当前所在路径，需记录当前的工作路径
```
![path](/lab1/dol2.png)



### 5.编译dol

进入刚刚dol的文件夹

```
$  cd../dol                    //进入dol的文件夹
```

修改build_zip.xml文件

找到下面这段话，就是说上面编译的systemc位置在哪里，

```
<property name="systemc.inc" value="YYY/include"/>
<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
```

把YYY改成上页pwd的结果（注意，对于  64位 系统的机器，lib-linux要改成lib-linux64）

然后是编译

```
$	ant -f build_zip.xml all
```

若成功会显示build successful

### 6.运行第一个例子

接着可以试试运行第一个例子

进入build/bin/main路径下

```
$	cd build/bin/main
```

然后运行第一个例子

```
$	ant -f runexample.xml -Dnumber=1
```

成功结果如图

![runexample](/lab1/dol3.png)

## 三、实验心得

​	本次实验主要是学习git版本控制，好处是在长期的代码修改周期中，它可以帮助我们实现修改代码但又不会原来的代码，而且在每次提交的代码日志当中还可以准确的知道修改的地方，方便了解版本的不同或者回到之前的版本。

​	另外需要学习的是markdown的制作。markdown也有自己的一套语法，但它通俗易懂，很容易排版和修饰内容，如果掌握好的话，在写作中非常流畅优雅。并且网上有许多markdown写作软件和网络编辑器，也可以帮我们轻松的完成markdown编辑。

