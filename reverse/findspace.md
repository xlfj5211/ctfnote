---
layout: default
---
# ![](../img/hj.jpg)找代码存放空间
>小贱提示： 如果代码量不大，可以放在区块间隙中，像SMC补丁那样。如果代码量大，需要再创建一个区块
>
>下面介绍三种方法

## ![](../img/github7.png)利用区块间隙
## ![](../img/github8.png)手工构造区块
```
增加块头->增加块头指向的数据段->调整映像尺寸
详情见加密与解密536页，一般都是用工具
```
## ![](../img/github9.png)手工构造区块
```
CFF explorer：
1、节头部中选中一个区块，右键添加节（空白区），大小随便，如1000h，然后填上节名字.hijack
2、右键，重建映像大小，保存即可
```
# ![](../img/hj.jpg)
>小贱提示：
>有些软件块表后的空间没有富余，不能增加区块了，如windows自带的一些程序，块表后紧跟着就是BoundImport数据。
>
>解决办法：
>
>在数据目录表里将BoundImport的RVA和Size赋0，同时将BoundImport数据区清零，处理后就可以正常添加区块了



__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
