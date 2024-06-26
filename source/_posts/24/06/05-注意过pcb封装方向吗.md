---
title: 注意过PCB封装图形的方向吗？
date: 2024-06-05 14:40:29
categories:
tags:
    - diy
---

在电子设计自动化软件（ECAD）中制作 PCB（Printed Circuit Board，印刷电路板）封装图形时，一个常见但容易被忽视的问题是图形的方向。这似乎是微不足道的细节，但在半导体和表面贴装技术（SMT）工业中，方向的选择可以对设计的性能、生产效率和成本产生重大影响。

通常情况下，在绘制PCB封装图形时，会以器件数据表中给的封装图形作为参考，图形方向也随之确定。比如之前介绍过的[一款国产DCDC降压芯片M3406-ADJ](https://mp.weixin.qq.com/s?__biz=MzA3NzMyNTIyOA==&mid=2651480821&idx=1&sn=eb674f67319aa27b080bb49bb6140e3e&chksm=84ad7436b3dafd20e4f95ceff2c3188af043e14f81e0a6d506dda171a56586b444f07126300d#rd)，在数据表中，厂商并没有给出推荐封装，只在最后给出了芯片尺寸图：

![M3406芯片尺寸图](https://imgs.boringhex.top/blog/20240605152719.png)

<!-- more -->

多数同学会按照这个图中的芯片方向来绘制封装图形。

那到底应该如何确定PCB封装图形的方向呢？

其实是有标准的，业界广泛遵循的标准是IPC-7351/SM-782A和EIA-481/EIA-783。

IPC和EIA是两个在电子工业中广泛使用的标准制定组织。

IPC，全称Institute of Printed Circuits，是一个全球性的非营利性贸易协会，致力于电子连接产业的竞争力和财务成功。IPC制定了许多电子组装和印刷电路板设计、制造和测试的标准。IPC-7351是一个关于表面贴装封装和焊盘设计的标准。

EIA，全称Electronic Industries Alliance，是一个美国的标准制定组织，它制定了许多关于电子和电气产品的标准。在你提到的EIA-481和EIA-783中，EIA-481是一个关于表面贴装元件卷带包装的标准，而EIA-783是一个关于表面贴装元件的尺寸和公差的标准。

这些标准对于保证产品的**质量**和**互操作性**非常重要。

这里特别强调一下互操作性，因为在实际的电子设计和生产中，尤其是现代以芯片为核心的设计中，从设计到生产制造，涉及到非常多的环节，只要大家都遵循一致的标准，才能保证高效的协作。而封装图形就是PCB设计和PCBA制造之间的一个重要环节。正确一致的设计可以减少制造过程很多不必要的麻烦，提高生产效率。

## 背景知识

### PCB 封装图形

PCB 封装图形是描述芯片、电容、电阻等元器件在 PCB 上的物理布局和连接的图形表示。它们通常包括引脚位置、尺寸、引脚间距、焊盘位置等信息。正确设计 PCB 封装图形对于确保电路板的性能和可靠性至关重要。

### 半导体工业

半导体行业是制造芯片的行业，这些芯片是电子设备的核心部件。在芯片制造过程中，封装是最后一个阶段，将芯片封装在外壳中，以便与其他元器件进行连接。对PCB设计来讲，除了跟芯片封装打交道，还要注意芯片的包装，比如现在贴片元件最流行的卷带包装。其实PCB封装图形的方向跟芯片的包装方向是密不可分的。

### SMT 工业

SMT 是一种电子组装技术，通过将元器件直接焊接到 PCB 表面上，而不是通过传统的插入孔焊接技术。SMT 技术能够提高生产效率和组件密度，因此在现代电子制造中得到广泛应用。

关于PCB封装图形方向的标准全文比较长，这里就列出关键原则：

1. 芯片电容器、电阻器和电感器 (RES, CAP 和 IND) - Pin 1 (正极) 在左边
2. 塑封电感器 (INDM)、电阻器 (RESM) 和钽电容器 (CAPT) - Pin 1 (正极) 在左边
3. 精密绕线电感器 (INDP) - Pin 1 (正极) 在左边
4. MELF二极管 - Pin 1 (阴极) 在左边
5. 铝电解电容器 (CAPAE) - Pin 1 (正极) 在左边
6. SOT设备 (SOT23, SOT23-5, SOT223, SOT89, SOT143等) - Pin 1 在左上角
7. TO252 & TO263 (DPAK类型)设备 - Pin 1 在左上角
8. 小外形鸥翼形IC (SOIC, SOP, TSOP, SSOP, TSSOP) - Pin 1 在左上角
9. 陶瓷平面封装 (CFP) - Pin 1 在左上角
10. 小外形J引脚IC (SOJ) - Pin 1 在左上角
11. 四边平面封装IC (PQFP, SQFP) - Pin 1 在左上角
12. 陶瓷四边平面封装 (CQFP) - Pin 1 在左上角
13. 凸缘四边平面封装IC (BQFP Pin 1 Center) - Pin 1 在顶部中心
14. 塑料引脚芯片载体 (PLCC) - Pin 1 在顶部中心
15. 无引脚芯片载体 (LCC) - Pin 1 在顶部中心
16. 芯片阵列 - Pin 1 在左上角
17. 无引脚四边平面封装IC (QFN) QFNS, QFNRV, QFNRH - Pin 1 在左上角
18. 球栅阵列 (BGA) - Pin A1 在左上角

以上是最主要的原则，但是在实操层面上，各家厂商并不完全一致，IPC标准和EIA标准也有一些差异，有些厂商遵循IPC，有些厂商则遵循EIA，所以在实际设计中，如果数据表中没有明确指出，最好跟制造上进行确认。

回到前文的M3406芯片封装，SOT23-5封装的芯片，按照上面的原则，Pin 1应该在左上角，跟数据表中示意图的方向是一致。这里要说明下"左上角"的意思，这个左上角并非画封装图形时屏幕左上角，而是指载带水平放置，盖膜朝上，传送方向向右时，芯片放置槽坑的左上角。很多厂商会以这个方向按照"Z"型标记Q1、Q2、Q3、Q4，Q1就是左上角，Q3是左下角。

而实际在画PCB封装图形时，封装图形对应的是载带向上传输的芯片的方向，也就是要按照前面说的逆时针转90度，这样Q1在屏幕上是左下，Q3是右下。

但实际上，M3406芯片的1脚是Q3，并没有按照前面的标准😳，在软件里画出来应该是这样的：

![SOT23-5封装图形](https://imgs.boringhex.top/blog/20240605164317.png)

![缩略图](https://imgs.boringhex.top/blog/20240605164105.png)

这个问题是这么来的：

![SOT23 vs MO-178](https://imgs.boringhex.top/blog/20240605164518.png)

所以呢，有些SOT23-5封装的芯片，有些厂商1脚在Q1，有些厂商1脚在Q3，在使用时多加注意吧！

PCB 封装图形方向虽然是一个看似微小的问题，但它对电路设计和生产过程有着重要的影响。设计是面向制造的，其实只要去看一下贴片机的工作过程，就很容易理解这个PCB封装图形方向的问题，希望这篇文章能给大家带来一点启发。希望我们下次画的板子，贴片时不再需要生产工程师逐个调整器件方向了。

最后贴一些常见封装图形的方向吧：

![](https://imgs.boringhex.top/blog/20240605165540.png)

![](https://imgs.boringhex.top/blog/20240605165617.png)