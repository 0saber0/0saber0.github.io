---
layout: post
title: JavaSE-Java的数据精度丢失
date: 2018-06-28 10:31:20
categories: JavaSE
tags: JavaSE float double
author: 密拉诺
---

* content
{:toc}

Java的八大基本数据类型中，**float**和**double**可以表示小数，分别叫做**单精度浮点数（float）**与**双精度浮点数（double）**。两者有以下区别：


1. 在内存中占有的字节数不同

 - 单精度浮点数在机内占4个字节

 - 双精度浮点数在机内占8个字节

2. 有效数字位数不同

 - 单精度浮点数有效数字8位

 - 双精度浮点数有效数字16位

3. 所能表示数的范围不同

 - 单精度浮点的表示范围：-3.40E+38 ~ +3.40E+38

 - 双精度浮点的表示范围：-1.79E+308 ~ +1.79E+308

4. 在程序中处理速度不同一般来说，CPU处理单精度浮点数的速度比处理双精度浮点数快

两个类型在数据的四则运算中是否会丢失精度呢？

## 1. float的加减乘除

测试代码：

```java
package com.milanuo;

/**
 * @author 密拉诺
 */
public class FloatDemo {

	public static void main(String[] args) {
		float a = 0.15f;
		float b = 0.3f;

		System.out.println("float的加法运算： " + a + "+" + b + "=" + (float)(a + b));
		System.out.println("float的减法运算： " + a + "-" + b + "=" + (float)(a - b));
		System.out.println("float的乘法运算： " + a + "*" + b + "=" + (float)(a * b));
		System.out.println("float的除法运算： " + a + "/" + b + "=" + (float)(a / b));

	}
}
```
测试结果：

```java
float的加法运算： 0.15+0.3=0.45000002
float的减法运算： 0.15-0.3=-0.15
float的乘法运算： 0.15*0.3=0.045
float的除法运算： 0.15/0.3=0.5
```
这个结果是不是偶然呢？我们多测试几组数据：
```java
float的加法运算： 2.4+1.2=3.6000001
float的减法运算： 2.4-1.2=1.2
float的乘法运算： 2.4*1.2=2.88
float的除法运算： 2.4/1.2=2.0

float的加法运算： 6.3+3.0=9.3
float的减法运算： 6.3-3.0=3.3000002
float的乘法运算： 6.3*3.0=18.900002
float的除法运算： 6.3/3.0=2.1000001

float的加法运算： 8.4+2.1=10.5
float的减法运算： 8.4-2.1=6.2999997
float的乘法运算： 8.4*2.1=17.639997
float的除法运算： 8.4/2.1=4.0
```
从上面的测试结果不难看出：
 - float在进行四则运算时存在精度损失
 - 测试的数据不同，表现出精度损失的运算不同

## 2. double的加减乘除

测试代码：

```java

package com.milanuo;

/**
 * @author 密拉诺
 */
public class DoubleDemo {

	public static void main(String[] args) {
		double a = 0.15D;
		double b = 0.3D;

		System.out.println("double的加法运算： " + a + "+" + b + "=" + (double)(a + b));
		System.out.println("double的减法运算： " + a + "-" + b + "=" + (double)(a - b));
		System.out.println("double的乘法运算： " + a + "*" + b + "=" + (double)(a * b));
		System.out.println("double的除法运算： " + a + "/" + b + "=" + (double)(a / b));

	}
}

```

测试结果：

```java

double的加法运算： 0.15+0.3=0.44999999999999996
double的减法运算： 0.15-0.3=-0.15
double的乘法运算： 0.15*0.3=0.045
double的除法运算： 0.15/0.3=0.5

double的加法运算： 2.4+1.2=3.5999999999999996
double的减法运算： 2.4-1.2=1.2
double的乘法运算： 2.4*1.2=2.88
double的除法运算： 2.4/1.2=2.0

double的加法运算： 6.3+3.0=9.3
double的减法运算： 6.3-3.0=3.3
double的乘法运算： 6.3*3.0=18.9
double的除法运算： 6.3/3.0=2.1

double的加法运算： 8.4+2.1=10.5
double的减法运算： 8.4-2.1=6.300000000000001
double的乘法运算： 8.4*2.1=17.64
double的除法运算： 8.4/2.1=4.0

```

相同的四组数据，在double类型中与float类型有所不同：
 - double在进行四则运算时也存在精度损失
 - double四则运算中精度损失集中表现在加减法，也可能是测试数据不够

## 3. 为什么会存在精度损失

绝大多数现代的计算机系统采纳了所谓的浮点数表达方式。这种表达方式利用科学计数法来表达实数，
即用一个尾数（Mantissa ），一个基数（Base），一个指数（Exponent）以及一个表示正负的符号来表达实数。
比如*123.45*用十进制科学计数法可以表达为*1.2345 × 10²*，其中**1.2345**为尾数，**10**为基数，**2**为指数。

浮点运算很少是精确的，只要是超过精度能表示的范围就会产生误差。往往产生误差不是因为数的大小，而是因为数的精度。
因此，产生的结果接近但不等于想要的结果。尤其在使用 float 和 double 作精确运算的时候要特别小心。

## 4. 总结

float和double类型主要是为了科学计算和工程计算而设计的。他们执行二进制浮点运算，
是为了在广泛的数字范围上提供较为精确的快速近似计算而精心设计的。
然而，它们并没有提供完全精确的结果，所以我们不应该用于精确计算的场合。

float和double类型尤其不适合用于货币运算，当我们需要精确的计算或者用于货币运算时，可以使用java.math.BigDecimal。