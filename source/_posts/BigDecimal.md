---
title: BigDecimal
date: 2019-03-19 00:04:35
mathjax: true
tags:
- 原创
categories:
- 技术文档
- java语言
- java
---

<!-- more -->

## 介绍

## 快速开始
```java
BigDecimal bigDecimal1 = new BigDecimal(1);
BigDecimal bigDecimal2 = new BigDecimal(2);

// 加法 bigDecimal1.add(bigDecimal2)
System.out.println(bigDecimal1.add(bigDecimal2));

// 减法 bigDecimal1.subtract(bigDecimal2)
System.out.println(bigDecimal1.subtract(bigDecimal2));

// 乘法 bigDecimal1.multiply(bigDecimal2)
System.out.println(bigDecimal1.multiply(bigDecimal2));

// 除法 bigDecimal1.divide(bigDecimal2, 4, RoundingMode.HALF_UP)
System.out.println(bigDecimal1.divide(bigDecimal2, 4, RoundingMode.HALF_UP));
```

#### RoundingMode
header 1 | header 2
---|---
ROUND_CEILING | 如果 BigDecimal 是正的，则做 ROUND_UP 操作；如果为负，则做 ROUND_DOWN 操作。
ROUND_DOWN | 从不在舍弃(即截断)的小数之前增加数字。
ROUND_FLOOR | 如果 BigDecimal 为正，则作 ROUND_UP ；如果为负，则作 ROUND_DOWN 。
ROUND_HALF_DOWN|若舍弃部分> .5，则作 ROUND_UP；否则，作 ROUND_DOWN 。
ROUND_HALF_EVEN|如果舍弃部分左边的数字为奇数，则作 ROUND_HALF_UP ；如果它为偶数，则作 ROUND_HALF_DOWN 。
HALF_UP | 若舍弃部分>=.5，则作 ROUND_UP ；否则，作 ROUND_DOWN 。
ROUND_UNNECESSARY|该“伪舍入模式”实际是指明所要求的操作必须是精确的，，因此不需要舍入操作。
ROUND_UP|总是在非 0 舍弃小数(即截断)之前增加数字。

## 为什么要用 BigDecimal

## 源码

### 类说明
不可变的，任意精度带符号的十进制数。BigDecimal由任意精度整数非标度值(unscaledValue)和32位整数标度(scale)组成。如果是零或正数, 则标度(scale)是小数点右边的位数。如果是负数, 则这个数的非标度值(unscaledValue)乘以10的负标度 (scale) 次方。因此，BigDecimal表示的数字的值是 $(unscaledValue × 10^{-scale})$ 。

BigDecimal类提供用于算术、标度(scale)操作、舍入、比较、哈希和格式转换的操作。 `toString()` 方法提供BigDecimal的规范表示。

BigDecimal类让用户完全控制舍入行为。如果没有指定四舍五入模式，并且无法表示精确的结果，则抛出异常；否则，可以通过向操作提供适当的 `MathContext` 对象来按照选定的精度和舍入模式进行计算。不论发生何种情况，舍入控制采用八种舍入方式。使用该类中的Integer字段(如 `ROUND_HALF_UP`)表示舍入模式在很大程度上已经过时；应该使用 `RoundingMode` 的枚举值(例如 `RoundingMode.HALF_UP`)。


### 加法

### 减法

### 乘法

### 除法

### 求平方根
公式为：`x=(x+a/x)/2=r`，其中：`x`代表猜测数，猜测数随便取一个数，甚至可以等于`a`，`a`代表被求平方根的数，经过重复多次计算，`x`将不断接近`a`的平方根。示例如下：

计算过程 | 结果
---|---
(4 + 2 / 4) / 2 | 2.25
(2.25 + 2 / 2.25) / 2 | 1.56944..
(1.56944.. + 2/1.56944..) / 2 | 1.42189..
(1.42189.. + 2/1.42189..) / 2 | 1.41423..

```java
public static BigDecimal sqrt(BigDecimal value, int scale){
    BigDecimal num2 = BigDecimal.valueOf(2);
    int precision = 100;
    MathContext mc = new MathContext(precision, RoundingMode.HALF_UP);
    BigDecimal deviation = value;
    int cnt = 0;
    while (cnt < precision) {
        deviation = (deviation.add(value.divide(deviation, mc))).divide(num2, mc);
        cnt++;
    }
    deviation = deviation.setScale(scale, BigDecimal.ROUND_HALF_UP);
    return deviation;
}
```
> **原文来源：** CSDN用户 Override笑看人生
**原文链接：** [https://blog.csdn.net/...](https://blog.csdn.net/rwzhang/article/details/85038627)


### 格式化

## 源码
