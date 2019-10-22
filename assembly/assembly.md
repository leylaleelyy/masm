# 汇编语言程序设计

计算机的硬件组成结构

CPU：运算器，控制器，寄存器

主存储器

I/O接口

汇编语言抽象成为：寄存器，存储器地址和输入输出地址



## 寄存器（Register）

* 处理器内部的告诉存储单元

* 用于暂时存放执行过程中的代码和数据

* 透明寄存器：对应用人员不可见，不可用编程直接控制

* 可编程寄存器：有引用名称

* * 通用寄存器
  * 专用寄存器

  

| 通用寄存器                           | 专用寄存器                            |
| ------------------------------------ | ------------------------------------- |
| 16位：AX、BX、CX、DX、SI、DI、BP、SP | 标志寄存器FLAGS<br/>指令指针寄存器EIP |
| 8位：AH、AL、BH、BL、CH、CL、DH、DL  | 段寄存器：CS、DS、SS、ES、FS、GS      |



### 通用寄存器

* 处理器最常使用的整数通用寄存器
* 可用于保存整数数据、地址等

 <img src="file:///C:\Users\Lenovo\Documents\Tencent Files\941565516\Image\C2C\{94C58A26-CC0B-C8B2-95D4-A5512BF5AFE9}.png" width = "200" height = "200" div align="center" />





| AX   | 累加器         | Accumulator       |
| ---- | -------------- | ----------------- |
| BX   | 基址寄存器     | Base Address      |
| CX   | 计数器         | Counter           |
| DX   | 数据存储器     | Data              |
| SI   | 源变址寄存器   | Source Index      |
| DI   | 目的变址寄存器 | Destination Index |
| BP   | 基址指针       | Base Pointer      |
| SP   | 堆栈指针       | Stack Pointer     |

 

### 处理器专用寄存

#### 标志Flag

* 标志体现了某种工作形态
* 有些处理器标志用以反映指令执行结果
* * 加减是否借位，数据是否为零，或者是正或负

* 有些处理器标志用于控制指令执行形式
* * 处理器是否单步操作、是否影响外部中断
* 设计一个或多个二进制表示一种标志
* 用0和1的组合表达标志的不同状态



##### 处理器最基本的标志：状态标志

* 用于记录指令执行结果的辅助星系
* 加减运算和逻辑运算指令主要设计他们
* 其他指令的执行也会相应的设置他们
* 处理器主要使用其中5个构成各种条件
* * 分支指令判断这些条件实现程序分支



#### 指令指针寄存器IP

保存将要**执行的指令在主存的存储器地址**

* 顺序执行时自动增量（加上该指令的字节数），**指向下一条指令**
* 分支、调用等操作时执行控制转移指令修改，引起程序转移到指定的指令执行
* 出现中断或异常时被处理器赋值而相应改变

保存一个特定用途的内容，一条指令

改变以改变指令执行的顺序

### 存储空间分段管理

“段”是保存相关代码或数据的一个主存区域

应用程序主要分3类基本段

| 主存空间                 |                                              |
| ------------------------ | -------------------------------------------- |
| 代码段(Code Segment) CS  | 存放程序的可执行代码（处理器指令）           |
| 数据段(Data Segment) DS  | 存放程序所用的数据，eg：全局变量             |
| 堆栈段(Stack Segment) SS | 程序需要的特殊区域，存放返回地址、临时变量等 |

段的说明开始——段寄存器，某个段在主存的位置

#### 段寄存器

表明某个段在主存中的位置

CS、SS、DS、ES



#### 代码段的当前指令地址

**代码段**

* 段基地址：代码段寄存器CS指示
* 偏移地址：指令指针偏移地址IP保存

存储器地址在编程时，是以逻辑地址形式访问

逻辑地址-> 段基地址：偏移地址

CS段基地址指明了代码段的开始

IP保存的偏移地址，明确指明正在执行的段内的哪个指令

组合指明当前指令地址

<img src="file:///C:\Users\Lenovo\Documents\Tencent Files\941565516\Image\C2C\{A3EC608F-28F5-3F97-FFDC-810F397D50C9}.png" width = "200" height = "200" div align=left />



####   堆栈段的当前栈顶地址

**堆栈段**

* 段基地址：堆栈段寄存器SS指示
* 偏移地址：堆栈指针寄存器SP保存

<img src="file:///C:\Users\Lenovo\Documents\Tencent Files\941565516\Image\C2C\{812CD6D1-F099-932C-62EC-0CB1A950C462}.png" width = "200" height = "200" div align=left />



#### 数据段的操作数地址

**数据段**

* 段基地址：数据段寄存器DS指示，有时也可ES、FS、GS指示
* 偏移地址：存储器寻址方式计算出有效地址EA指示



## 存储器组织

被抽象为存储器地址

### 存储器地址

主存储器容量很大、被划分为许多存储单元

每个存储单元被编排一个号码、即存储单元地址

* 称为存储器地址

每个存储单元以字节为基本存储单位

* 字节编址（Byte Addressable）
* 一个字节=八个二进制位：1Byte=8Bit

数据的基本单位：位、字节、字、双字

### 存储器的物理地址

处理器连接的物理存储器使用物理地址

* 从0开始
* 直到其支持的最大存储单元

#### 存储模型

程序并不直接寻址物理存储器，会对存储器的管理有麻烦

MMU存储管理单元，存储模型，用于程序访问存储器

### 逻辑地址

存储器空间可以分段管理，采用逻辑地址指示

* 逻辑地址=段基地址：偏移地址
* * 段基地址=在主存中的起始位置
  * 偏移地址=距离段基地址的位移量
* 处理器内部以及编程时采用逻辑地址

物理地址是唯一的，而逻辑地址可以多个



## 处理器指令

指令由操作码和操作数组成

* 操作码表明处理器执行的操作
* * 数据传送、加法、跳转等
  * 指令注记符表示
* 操作数是参与操作的数据对象
* * 主要以寄存器名或地址形式指明数据的来源
  * 使用寄存器、常量、变量等形式表示

### MOV

传送指定：`MOV`

将数据从一个位置传送到另一个位置，类似高级语言的赋值语句

```assembly
mov dest,src ;dest<-src
;目的操作数dest：数据将要传送到的位置；源操作数src：被传送的数据或数据所在的位置
mov ax,100 ;AX<-100(常量)
mov ax dvar;AX<-dvar(变量)
mov ax bx ;AX<-bx(寄存器)
```

给定的是操作数的位置，实际传输的操作数位置的数据



## 语句格式

源程序由语句组成

* 执行性语句：表达处理器指令、实现功能

  标号：	硬指令注记符	操作数，操作数 ;注释

* 说明性语句：表达伪指令、控制汇编方式

  

  

  