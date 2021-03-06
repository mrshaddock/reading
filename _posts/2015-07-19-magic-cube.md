---
layout: post
title: 三阶魔方还原教程
categories: 当然我在扯淡
tags: 三阶魔方
---

前阵子买了个魔方，一直处于闲置状态。今天下雨比较嗨，就拿出来摆弄了几下。但是悲剧的发现以我的智商完全玩不转T_T只好 google 了一下魔方教程，花了1个小时就“学会”了。之所以加引号，是因为只死记硬背了几个公式。。不看教程的情况下自己玩了几次，然后就喜闻乐见的记混了= =于是把还原过程涉及的步骤都记下来，忘的时候复习复习。

当然，感觉这样玩法太 low ，纯粹是背诵公式，对魔方没有整体的认识，也没有思考为什么要这么旋转。但既然是初学嘛，先玩着再说，熟练以后再慢慢想象魔方是怎么旋转的。另外google 了一下，最牛逼的选手8秒左右就能还原一个三阶魔方，简直不可思议。。。虽然知道有很多牛逼的公式，但还是一步一步来吧。By the way, 终于可以愉快的玩(zhuang)耍(bi)啦~~~~~~~

### 魔方介绍

三阶魔方一共6个面，每个面颜色相同。由8个角色块(每个角块是3面颜色)、12个棱色块(每个棱块是2面颜色)、6个中心块三部分组成。虽然只有区区6+8+12=26个小方块，但是它的总变化数高达4.3乘以10的19次方。什么概念呢？假如一秒可以转动三下魔方，不重复任何一种变化，需要4542亿年才能遍历所有的魔方组合。

对一个标准魔方而言，中心块的相对位置是固定的，肯定是红橙相对，蓝绿相对，黄白相对，某个面的中心块是什么颜色，还原魔方后这个面就会是什么颜色。最简单的还原方法是层序法，就是一层一层还原，我找的教程就是先还原底层、然后是中间层，最后是顶层。其中只需要记住一个公式：

> **上内下外：上下指最左面或者最右面，内外是指顶层。**很简单，当从右边操作，就是最右面向上90°，顶层向内（顺时针）90°，最右边向下90°，顶层向外（逆时针）90°。左边同理。

### 底层

#### 1. 底面十字

将底面拼成一个十字块，没啥难度。

#### 2. 和侧面中心块同色

1. 同色是对面：一个朝后，右公式+右上，变成情况2
2. 同色是相邻面：前左，右公式+右上

#### 3. 底层角块

将底面十字颠倒为底层，然后开始调整底面四个角块的还原。

1. 找到和底面颜色相同的角块（在顶层四个角或者底层四个角）
2. 转到角块其他两个颜色的中心块所在面的位置
3. 根据角块在顶层或者底层的位置：
    * 底层底色朝前：右公式4次
    * 底层底色朝右：右公式2次
    * 顶层底色朝右：右公式1次
    * 顶层底色朝前：魔方左转，左公式1次
    * 顶层底色朝上：右公式3次

### 第二层

#### 1. 还原4个棱块

1. 首先找顶层中不含底色对面颜色的棱块，并旋转到对应颜色的中心块的面，若棱块另一个颜色：
	* 在左面：顶层右转90°，左公式1次，魔方右转，右公式1次
	* 在右面：顶层左转90°，右公式1次，魔方左转，左公式1次
2. 若找不到顶层中不含底色对面颜色的棱块（2种特殊情况：棱块有2个位置不正确或者4个棱块位置正确但有一个色相相反）：右公式1次，魔方左转，左公式1次

其实所有操作都是把第二层的棱块和顶层的棱块交换位置

### 顶层

#### 1. 顶面十字

针对顶层中间块，有三种情况：

1. 顶面中心块上下左右没同色：最前面顺时针90°，右公式1次，最前面逆时针90°，变成情况2
2. 顶面中心块有拐弯的2个同色：2个同色是左下或者右下，最前面顺时针90°，右公式1次，最前面逆时针90°，变成情况3
3. 顶面中心块有直线的2个同色：3个同色横着，最前面顺时针90°，右公式1次，最前面逆时针90°，完成顶面十字

#### 2. 和侧面中心块同色

将顶面棱块转到和侧面中心块同色后，有两种情况：

1. 两个同色在对面：同色在前、后，最前面顺时针90°，右公式2次，最前面旋转180°（正反一样），左公式1次，最前面顺时针90°，变成情况2
2. 两个同色相邻：同色在左、后，最前面顺时针90°，右公式2次，最前面旋转180°（蒸饭一样），左公式1次，最前面顺时针90°，完成后旋转顶层即可同色

#### 3. 顶层角块位置

调整顶层的4个角块位置，要让角块的三个颜色和所在角的三个面颜色一致。这时候有三种情况：

1. 4个角块位置都不对：右公式3次，最左边向上90°，右公式3次，最左边向下90°，变成状态2
2. 1个角度位置对：我们先要看其他3个角块要调整位置的转向：
    * 顺时针：位置对的角块在左手：左公式3次，最右边向上90°，左公式3次，最右边向下90°
    * 逆时针：位置对的角块在右手：右公式3次，最左边向上90°，右公式3次，最左边向下90°

#### 4. 色相

顶层面朝左，随机找顶层需要还原的角块，移动至左后方：

1. 顶层颜色向上：右公式4次
2. 顶层颜色向后：右公式2次