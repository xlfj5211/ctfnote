---
layout: default
---
# ![](../img/hj.jpg)IL算数指令及方法调用指令


## ![](../img/github6.png)算数指令
#### 堆栈操作

| 指令 | 说明                               |
| ---- | ---------------------------------- |
| nop  | 空指令                             |
| dup  | 复制当前栈顶元素，若栈顶为空则出错 |
|  pop    |    出栈，栈顶为空时出错                                |

#### ldc:载入常数

| 指令                   | 说明                  |
| ---------------------- | --------------------- |
| ldc.i4 <int32>         | 载入<int32>           |
| ldc.i4.s <int8>        | 载入<int8>            |
| ldc.i4.m1 (ldc.i4.M1） | 载入-1                |
| ldc.i4.0               | 载入0                 |
| ldc.i4.1               | 载入1                 |
| 以此类推               | 以此类推              |
| ldc.i4.8               | 载入8                 |
| ldc.r4 <float32>       | 载入<float32>(单精度) |
|      ldc.r8 <float64>                  |      载入<float64>(双精度)                 |

#### ldind：间接载入
>小贱提示：
>
>从栈顶取出一个指针，载入该指针所指向的数值，并入栈，小数点后的名称表明载入数据的类型

| 指令      | 说明                                     |
| --------- | ---------------------------------------- |
| ldind.i1  | 载入1字节有符号整数                      |
| ldind.i2  | 载入2字节有符号整数                      |
| ldind.i4  | 载入4字节有符号整数                      |
| ldind.i8  | 载入8字节有符号整数                      |
| ldind.u1  | 载入1字节无符号整数                      |
| ldind.u2  | 载入2字节无符号整数                      |
| ldind.u4  | 载入4字节无符号整数                      |
| ldind.u8  | 载入8字节无符号整数                      |
| ldind.i   | 载入native int，大小为当前系统的指针大小 |
| ldind.r4  | 载入单精度浮点数据                       |
| ldind.r8  | 载入双精度浮点数据                       |
| ldind.ref | 载入对象引用                             |

#### stind：间接存储
>小贱提示：
>
>从栈顶先后取一个值和一个指针，并将该值存储至指针所指位置

| 指令      | 说明                                     |
| --------- | ---------------------------------------- |
| stind.ref |    存储一个对象引用|
| stind.i1  |    存储1字节整数|
| stind.i2   |   存储2字节整数|
| stind.i4  |    存储4字节整数|
| stind.i8   |   存储8字节整数|
| stind.i   |     存储一个指针大小的数据|
| stind.r4 |     存储单精度浮点数|
| stind.r8   |   存储双精度浮点数|

#### 算数运算
>小贱提示：
>
>从栈顶取两个参数进行相应运算，并将结果入栈

add: 加法

sub: 减法

mul: 乘法
```
对于浮点数，存在两个特殊值
无限(infinity)和NaN(Not a Number),且规定0 * infinity = NaN
```

div: 除法
```
div.un 无符号整数相除
整数除以0抛出DivideByZero异常
```
浮点数特殊：
```
0/0=NaN
infinity/infinity=NaN
x/infinity=0
```

rem: 模运算
```
rem.un 无符号整数的模运算

整数运算时模0抛出DivideByZero异常
浮点数特殊：
infinity rem x = NaN
x rem 0 = NaN
x rem infinity=x
```
neg: 正负号变换(特殊指令，仅从堆栈取一个数值)

![](../img/hj.jpg)
>小贱提示：
>
>带溢出算数运算同上，只是溢出时抛出Overflow异常,如：
>
>add.ovf,add.ovf.un等等

#### 位操作
```
and   or   xor    not

shl 左移位

shr 右移位

shr.un无符号右移位

一条not相当于：
ldc.i4.1
add
neg
```

#### 转换指令
conv
#### 逻辑条件检测
>小贱提示：
>
>与比较跳转类似，但不跳转，仅将计算结果入栈（1：真，0：假）
>
>它从栈上取两个数据,<value2>为栈顶元素

| 指令 | 说明 |
| ---- | ---- |
| ceq| 检测二者是否相等|
| cgt| 检测<value1>是否大于<value2>|
| cgt.un| 无符号大于检测|
| clt| 检测<value1>是否小于<value2>|
| clt.un| 无符号小于检测|
| ckfinite| 单操作数指令，仅取栈顶元素，如果该数据为+infinity,-infinity或NaN，则抛出异常，否则将原数据入栈|

#### 块操作
cpblk
```
复制一块内存，按顺序从堆栈取三个操作数：
           待复制内存块的字节大小
           源地址
           目标地址
```

initblk
```
初始化一块内存，按顺序从堆栈取三个操作数：
           待初始化内存块的字节大小
           初始化值
           块的起始地址
```

## ![](../img/github7.png)方法调用指令
#### 直接调用

| 指令 | 用法        |
| ---- | ----------- |
| jmp  | jmp <token> |
|  call    |    call <token>         |
|  callvirt    |      callvirt <token>       |

>小贱提示：
>
>callvirt <token> ：通过实例v-token调用<token>所指的方法

#### 间接调用

| 指令              | 说明                                                                  |
| ----------------- | --------------------------------------------------------------------- |
| ldftn <token>     | 取得<token>所标示方法的指针并入栈                                     |
| ldvirtftn <token> | 从栈顶取对象引用（实例指针），并从相应的v-table中取得方法的指针并入栈 |
| calli <token>     |       从栈顶获取方法的指针，并从栈中依次读取方法的参数，按<token>所标示的方式调用方法。                                                                            |


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
