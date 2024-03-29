---
title: diy遥控飞机模型的基本要点-2
date: 2023-06-09 12:03:26
categories:
tags:
    - diy
    - 硬件
---

## 电动马达

在改装电动遥控飞机时，选择合适的马达和螺旋桨是非常重要的。以下是一些建议：

1. 马达选择：选择适合的马达需要考虑飞机的重量、翼展和预期性能。对于48cm翼展的手抛泡沫飞机，一般来说，你可以选择一款轻量级的无刷直流电机（BLDC）马达。根据飞机的重量和所需推力，选择合适的马达型号和KV值。通常，较低的KV值适合大直径的螺旋桨和较低的飞行速度，而较高的KV值适合小直径的螺旋桨和较高的飞行速度。

2. 螺旋桨选择：螺旋桨的选择与马达相互关联，应根据马达的KV值、推力要求和飞机的设计特点来确定。根据马达制造商的推荐或测试数据，选择适当的螺旋桨直径和螺距。一般来说，对于手抛飞机，可以选择较大直径的螺旋桨，以获得足够的推力和升力。确保螺旋桨的尺寸和形状与飞机的机身结构相匹配，以确保有效的推力传递和空气动力学性能。

3. 动力系统匹配：马达和螺旋桨的选择需要相互匹配，以获得最佳性能和效率。马达和螺旋桨的组合应使得马达在额定工作电压下能够提供足够的推力，并保持适当的工作温度。可以参考马达和螺旋桨制造商提供的技术规格和推荐组合，或者咨询飞行爱好者或专业人士的建议。

4. 调试和测试：在安装马达和螺旋桨后，进行必要的调试和测试，以确保动力系统的稳定性和安全性。检查马达的工作温度、螺旋桨的平衡以及推力的合适性。如果需要，进行必要的调整和优化。

重要的是，根据飞机的需求和设计参数进行合理的选择，并留有一定的余量。在飞行之前，确保所有电动系统和螺旋桨的安装牢固，并进行必要的安全检查和飞行前测试。对于48cm翼展手抛机，可以选择1104或相近规格的无刷电机，也可以选择720或820空心杯电机。

<!-- more -->

### 1104无刷电机

![1104无刷电机](https://imgs.boringhex.top/blog/20230605203123.png)

![1104无刷电机重量](https://imgs.boringhex.top/blog/20230605203233.png)

1104无刷电机的具体参数可以因不同的制造商和型号而有所差异，但通常包括以下几个方面的技术规格：

1. 尺寸：1104无刷电机的尺寸是指定子直径11mm，高度4mm。这是标准的1104电机尺寸，但可能会有一些微小的差异。

2. KV值：KV值是指每分钟电机旋转的转数与输入电压之间的比例关系，以转/伏（rpm/V）为单位。不同的1104无刷电机可能有不同的KV值可选，常见的KV值范围为4000KV到7500KV。较低的KV值适用于较大直径的螺旋桨和低速飞行，而较高的KV值适用于较小直径的螺旋桨和高速飞行。

3. 电压范围：1104无刷电机通常能够在一定的电压范围内正常工作。常见的电压范围为2S至4S锂电池（7.4V至14.8V），但具体范围可能因电机设计而异。在选择电池时，应确保其电压在电机规定的范围内。

4. 最大功率输出：每个1104无刷电机在其规格表中都会列出其最大功率输出。功率输出与电机的设计、材料和冷却系统有关。最大功率输出通常以瓦特（W）为单位表示。

5. 最大推力：1104无刷电机的最大推力也是一项重要参数。推力与螺旋桨的选择、电压和电机的功率输出有关。不同型号的1104无刷电机可以提供不同的最大推力。

这些参数仅供参考，具体的技术规格应参考所选电机的制造商提供的产品说明书或技术规格表。不同的1104无刷电机可能具有不同的性能和规格，因此在选择时应仔细查阅相关信息。

这款电机非常小巧，第一次看到这么小的无刷电机是在零度的dobby口袋无人机，个人感觉零度这款机型还不错，可惜最后还是没撑住，后来大疆也出了迷你折叠机型。

使用无刷电机的优点是大功率、大推力，缺点则是需要电调、电池要上2s，重量也相对较重。

### 720/820空心杯电机

![空心杯电机型号说明](https://imgs.boringhex.top/blog/20230605204331.png)

![820空心杯电机规格](https://imgs.boringhex.top/blog/20230605204544.png)

空心杯电机属于直流、永磁、同步电机，可以有刷也可无刷（不太常见）。因为没有铁芯，所以没有铁损，效率高。一般来说线圈作为转子，转动惯量小，好控制，效率高，转子线圈只能做得很薄，否则会损失磁通，所以空心杯电机功率一般做不大。空心杯电机两根线不分正负极，可实现电机正反转，接线后若电机转向不是您想要的，调换接线就可以了，使用时一定要先确定好转动方向，及时调整接线。

空心杯电机一般空载转速都很高，像上面这个820，3.7V时空载转速能到40000多，但是带载后转速会降很多。

使用空心杯电机的优点是重量轻，控制简单，不需要电调，缺点则是动力不是很强劲，之前用1s锂电池，720空心杯双发，感觉推48cm翼展手抛机不太轻松。

用空心杯电机的话，改装时电机座也比较好弄，最简陋的直接用热熔胶粘到机翼上就行，但是总感觉这样有飞着飞着掉发动机的风险😀😀😀😀😀😀😀😀😀😀😀😀😀😀😀😀😀

最好还是3d打印一个电机座，这样也必要好保持两侧对称性，拉力线或推力线一致性也比较容易保证。

要安装电机座，自然想到获取机翼3d模型，上网搜了下，3d扫描价格不菲，下面分别用两款app试了下：

<iframe src="//player.bilibili.com/player.html?bvid=BV1bu411W72D&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

<iframe src="//player.bilibili.com/player.html?bvid=BV1Wo4y1K71u&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

3d scanner app是基于结构光传感器，需要手机支持人脸识别。scaniverse则是基于计算机视觉，通过多角度大量拍照，然后计算得出模型。目前感觉在手机上预览效果尚可，但是导出的3d模型不太行，得到的都是多表面模型，感觉还需要针对点云数据做标片平滑处理才行，咱也不会啊！模型放下面链接了，要是有大佬知道该怎么处理，还望不吝赐教，留言给点提示也好~

链接: https://pan.baidu.com/s/1eMxM14TNJDxADggn2zK2QQ?pwd=hn8m 提取码: hn8m 复制这段内容后打开百度网盘手机App，操作更方便哦 
