---
layout:     post
title:      "BinarySearch的一些变种"
subtitle:   "upper lower ceil floor"
date:       2021-12-28 13:30
author:     "Xiaohan"
header-img: "img/code001.jpg"
category: Learning
catalog: true
tags:
    - Binary search
    - 数据结构
---

今天学习了一些BinarySearch的一些变种，方便在浮点数取正运算。但是对于一些名称的定义有些疑惑，查了一些资料没找到想要的答案，特意贴到这里供大家讨论

## 代码

### upper

在数组data中寻找大于target值的最小值的index

在合格的商品中找到最低品质

```java
// > target 的最小值
    // X X X [target] res X X
    // X X X [target-0.01] res X X
    public static <E extends Comparable<E>> int upper(E[] data, E target) {
        int l = 0, r = data.length;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (data[mid].compareTo(target) <= 0) {
                //data[0,mid]都小于等于target
                //有可能data[mid] == target; l = r = mid + 1;
                l = mid + 1;
            } else r = mid;
        }
        return l;
    }
```

### lower

在数组data中寻找小于target值的最大值的index

在不及格的学生中找到成绩最好的

```java
// < target 的最大索引值
    // X X X res [target] X X
    // X X X res [target+0.01] X X
    public static <E extends Comparable<E>> int lower(E[] data, E target) {
        int l = -1, r = data.length - 1;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (data[mid].compareTo(target) < 0) {
                //data[mid,data.length-1]都大于等于target
                //有可能data[mid] == target; l = r = mid - 1;
                l = mid;
            } else r = mid - 1;
        }
        return l;
    }
```

### upper_ceil

在数组data中寻找大于等于target值的最小索引

如果target存在，返回重复target的最大索引，如果target的不存在则返回upper

```java
// 存在target返回最大的索引
    // 不存在target返回upper（刚刚超过target的数字）
    // 上1法（五入法）
    //X X X [target] [target] [target_res] X X
    //X X X [target-0.01] [target-0.01] [target+0.01_res] X X
    //原名为ceil和lower_floor呼应（最顶上或者超过）
    public static <E extends Comparable<E>> int upper_ceil(E[] data, E target) {
        int u = upper(data, target);
        if (u - 1 >= 0 && data[u - 1].compareTo(target) == 0) {
            //如果upper左边的值和target一样，存在，u-1就是最大索引
            return u - 1;
            //如果不想等，则不存在，返回upper
        } else return u;
    }
```

### lower_floor

在数组data中寻找小于于等于target值的最大索引

如果target存在，返回重复target的最小索引，如果target的不存在则返回lower

```java
// 存在target返回最小的索引
    // 不存在target返回lower（刚刚超过target的数字）
    // X X X [target_res] [target] [target] X X
    // X X [target-0.01_res] [target+0.01] [target+0.01] X X
    // 与upper_ceil相呼应，最低或者小于
    public static <E extends Comparable<E>> int lower_floor(E[] data, E target) {
        int l = lower(data, target);
        if (l + 1 < data.length && data[l + 1].compareTo(target) == 0) {
            return l + 1;
        }
        return l;
    }
```

### 疑问

从上述的upper，lower，floor，ceil的实现方式来看，可以这样理解：

floor是所有target的最小索引，lower在floor向下一位

ceil是所有target的最大索引，upper在ceil向上一位

如果按照字面意思，把target值当作一个房子，那么floor为地板，即target最小索引，ceil为天花板，target最大索引，而upper和lower则分别代表了屋顶以上，和地板以下。

而对于两位两种组合upper_floor 和 lower_ceil 的定义和这个就完全相悖了


### upper_floor

target存在返回最大索引，不存在返回lower

```java
    // 存在target返回最大的索引
    // 不存在target返回lower（刚刚超过target的数字）
    // X X X [target] [target] [target_res] X X
    // X X X res [target+0.01] [target+0.01]  X X
    // 最高或者小于 (叫做lower_ceil更合适呢）
    public static <E extends Comparable<E>> int upper_floor(E[] data, E target) {
        int l = -1, r = data.length - 1;

        while (l < r) {
            int mid = l + (r - l) / 2;
            if (data[mid].compareTo(target) <= 0) {
                //data[mid,r]都小于等于target，不断向右边界逼近。
                //如果存在target则是在所有target值中的最右边
                //如果不存在则是小于target的右边界
                l = mid;
            } else r = mid - 1;
        }
        return l;
    }

```

### lower_ceil

target存在返回最小索引，不存在返回upper

```java
// 存在target返回最小的索引
    // 不存在target返回upper（刚刚超过target的数字）
    // X X X [target_res] [target] [target] X X
    // X X X [target-0.01] [target-0.01] res X X
    // 最低或者超过（应该叫做upeer_floor呢）
    public static <E extends Comparable<E>> int lower_ceil(E[] data, E target) {
        int l = 0, r = data.length;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (target.compareTo(data[mid]) > 0) {
                //只有在大于的时候才往右移动，如果相等，也是向左移动（比如5，5，5，5，5中查找5，无论是哪个五都会向左移动
                //直到移到5的左边，然后向右移动一位，则就是第一个5，最小的索引
                l = mid + 1;
            } else r = mid;
        }
        return l;
    }
```

### 奇怪

参考链接

https://its201.com/article/u013250861/109302494
https://blog.csdn.net/woaichikaoya/article/details/115742841

欢迎大家来讨论！！