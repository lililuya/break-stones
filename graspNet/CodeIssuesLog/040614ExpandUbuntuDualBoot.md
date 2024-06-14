---
typora-copy-images-to: img
typora-root-url: ./
---

<center><h4/>因为我ubuntu只分配了110G，内存不够了，便又给其加了50G</center>

#### 1.下载rufus

这玩意儿随便在网上找一个就OK的



#### 2.整启动盘

里面的参数照着我的修改就行

<img src="/img/0614_01.png" alt="0614_01" style="zoom:50%;" />

#### 3.压缩卷

我这里压缩50G，处于未分配状态就OK了

![0614_02](/img/0614_02.png)

#### 4.进入USB引导盘，分配卷

网上有参考，随便找一个就OK

我这里未分配的分区和linux系统中间隔了很多分区，这里就需要移动分区了

<img src="/img/0614_03.jpg" alt="0614_03" style="zoom: 33%;" />

这里做一个resize的操作，把forward转移到backward

<img src="/img/217c0fd631e6a883512b822c3ab2f6d7.jpg" alt="img" style="zoom: 33%;" />

最后一个就去掉前面的，然后分配给Linux分区，后续following分区也不分配就行

![img](/img/3c9577bea119c3df9393acb373d1520e.jpg)

后面点绿色对勾，结束了就OK

![img](/img/d2e3dde6dad3cba0960d3a456d667255.jpg)

#### 4.调整一下启动顺序，进入win和需要自己扩容的系统检查一下就OK

网上搜索就行

#### 5.参考文章

这个我觉得不错的，推荐一下

双系统扩容：https://blog.csdn.net/weixin_45055622/article/details/125674432

