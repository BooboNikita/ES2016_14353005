# DOL e1，e2实例分析&编程

## 一、dot截图
![example1](/lab2/example1.png)

            example1

![example2](/lab2/example2.png)

            example2

## 二、修改代码

### 1.example1

​	例子里包括生产者、平方模块、消费者三个模块，而题目要求将结果变为立方，所以根据模块名称，自然要修改的是平方模块。

![example1](/lab2/example1pic.png)

```
#include <stdio.h>

#include "square.h"

void square_init(DOLProcess *p) {
    p->local->index = 0;
    p->local->len = LENGTH;
}

int square_fire(DOLProcess *p) {
    float i;

    if (p->local->index < p->local->len) {
        DOL_read((void*)PORT_IN, &i, sizeof(float), p);
        i = i*i;
        DOL_write((void*)PORT_OUT, &i, sizeof(float), p);
        p->local->index++;
    }

    if (p->local->index >= p->local->len) {
        DOL_detach(p);
        return -1;
    }

    return 0;
}
```

​	在square.c当中，可以看见一个初始化函数，一个信号处理函数。如果当前位置小于长度，则可以执行程序，否则就将销毁实例。

​	在第一个if语句中，可以看见执行`i=i*i`语句，可见是平方的算法，为了修改为立方，可修改为`i=i*i*i` 即可。

### 2.example2

​	例2同样包含生产者、平方模块和消费者，不同的事中间连接了三个平方模块，产生的结果就变成了八次方。题目要求减为2个平方模块。而模块的控制是在xml文件里。

![example2](/lab2/example2pic.png)
```
<variable value="3" name="N"/>
  <!-- instantiate resources -->
  <process name="generator">
    <port type="output" name="10"/>
    <source type="c" location="generator.c"/>
  </process>
  <iterator variable="i" range="N">
    <process name="square">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
      <source type="c" location="square.c"/>
    </process>
  </iterator>
  <process name="consumer">
    <port type="input" name="100"/>
    <source type="c" location="consumer.c"/>
  </process>

  <iterator variable="i" range="N + 1">
    <sw_channel type="fifo" size="10" name="C2">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
    </sw_channel>
  </iterator>
  <!-- instantiate connection -->
  <iterator variable="i" range="N">
    <connection name="to_square">
      <append function="i"/>
      <origin name="C2">
        <append function="i"/>
        <port name="1"/>
      </origin>
      <target name="square">
        <append function="i"/>
        <port name="0"/>
      </target>
    </connection>
    <connection name="from_square">
        <append function="i"/>
        <origin name="square">
          <append function="i"/>
          <port name="1"/>
        </origin>
        <target name="C2">
          <append function="i + 1"/>
          <port name="0"/>
        </target>
    </connection>
  </iterator>
  <connection name="g_">
    <origin name="generator">
     <port name="10"/>
    </origin>
    <target name="C2"> 
      <append function="0"/>
      <port name="0"/>
    </target>
  </connection>
  <connection name="_c">
    <origin name="C2">
      <append function="N"/>
      <port name="1"/>
    </origin>
    <target name="consumer">
      <port name="100"/>
    </target>
  </connection>
</processnetwork>
```

​	经观察，发现平方模块是由迭代语句重复生成的，具体为：

```
<variable value="3" name="N"/>
  <!-- instantiate resources -->
  <process name="generator">
    <port type="output" name="10"/>
    <source type="c" location="generator.c"/>
  </process>
  <iterator variable="i" range="N">
    <process name="square">
      <append function="i"/>
      <port type="input" name="0"/>
      <port type="output" name="1"/>
      <source type="c" location="square.c"/>
    </process>
  </iterator>
```

​	可以看出迭代三次，所以产生了3个square模块，所以可以直接将value改为2，即`<variable value="3" name="N"/>`,或者将range改为N-1,即`<variable value="3" name="N"/>`，但第二种方法不符合变量的修改规则，只是单纯的实现了结果，所以最好不要用第二种。

## 三、实验感想

​	本次实验初步了解了dol里两个例子的组成结构以及运行实现过程，并进行了简单的代码修改，最后通过观察数值变化来验证是否实现，也接触了dot语言，利用语句进行流程图，结构图的绘制，这种绘制方法比图形界面更加准确和清晰，同时可以通过视图和语句来分析个中关系。
