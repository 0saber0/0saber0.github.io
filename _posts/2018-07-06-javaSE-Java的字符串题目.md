---
layout: post
title: JavaSE-Java的字符串题目
date: 2018-07-06 15:24:44
categories: String
tags: JavaSE String 
author: 密拉诺
---

* content
{:toc}

## 1. 去除重复并排序

### 1.1 没有分隔符

输入：           字符串


输出：           去除重复字符并排序的字符串


样例输入：       aabceeddf


样例输出：       abcdef



方法1：

```java
package com.milanuo;

import java.util.Scanner;
/**
 * @author 密拉诺
 */
public class Norepart {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("输入任意字符串：");
		String input = sc.nextLine();
		System.out.println(noRepeat(input));
		sc.close();
	}

	public static String noRepeat(String str) {
		char[] chars = new char[133];// 新建一个char数组，大小与char码值对应列表相同
		char[] input = str.toCharArray();// 将输入的字符串转换为char数组

		int temp;// 暂时存放char数组中的每一个数
		for (int i = 0; i < input.length; i++) {
			temp = input[i];// 将第i个输入的字符所对应的char数值赋给temp
			if (chars[temp] == 0) {// 新建的char数组存的都是默认的初始化值，即都是0
				chars[temp] = 1;// 将出现过的字符所对应的数组中的值变为1
			}
		}

		StringBuilder sb = new StringBuilder();
		for (int i = 0; i < chars.length; i++) {
			if (chars[i] == 1) {// 循环新建char数组，将数组中值为1的索引强转为char，并添加到StringBuilder中
				sb.append((char) i);
			}
		}
		return sb.toString();// 将StringBuilder转换为String返回
	}
}
```
方法2：

```java

package com.milanuo;

import java.util.Map;
import java.util.Scanner;
import java.util.Set;
import java.util.TreeMap;

/**
 * @author 密拉诺
 */
public class Norepart2 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("输入任意字符串：");
		String input = sc.nextLine();
		System.out.println(noRepeat(input));
		sc.close();
	}

	public static String noRepeat(String str) {
		char[] input = str.toCharArray();// 将输入的字符串转换为char数组

		Map<Character, Integer> map = new TreeMap<>();// 新建一个TreeMap
		for (int i = 0; i < input.length; i++) {
			map.put(input[i], 1);// 将转换后的char数组中的每一个char值存放在TreeMap中的key中，value可以随便写
									// 由于TreeMap中的key不允许重复，相同的值会覆盖
									// TreeMap中默认使用了自然排序
		}

		Set<Character> keySet = map.keySet();// 获取所有的key值
		StringBuilder sb = new StringBuilder(32);
		for (Character character : keySet) {
			sb.append(character);// 将每一个key值循环出来，拼接到StringBuilder中
		}

		return sb.toString();// 将StringBuilder转换为String返回
	}

}

```
### 1.2 有分隔符(以","为例)

与没有分隔符的区别不大，只是在将输入的String转换成char数组时不同：

```java

String[] strs = input.split(",");

```
别的做相应变化

```java
package com.milanuo;

import java.util.Map;
import java.util.Scanner;
import java.util.Set;
import java.util.TreeMap;

/**
 * @author 密拉诺
 */
public class Norepart3 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("输入任意字符串：");
		String input = sc.nextLine();
		System.out.println(noRepeat(input));
		sc.close();
	}

	public static String noRepeat(String str) {
		String[] input = str.split(",");// 将传过来的字符串按要求的分隔符转化成String[];

		Map<String, Integer> map = new TreeMap<>();// 新建一个TreeMap
		for (int i = 0; i < input.length; i++) {
			map.put(input[i], 1);
		}

		Set<String> keySet = map.keySet();// 获取所有的key值
		StringBuilder sb = new StringBuilder(64);
		for (String string : keySet) {
			sb.append(string);// 将每一个key值循环出来，拼接到StringBuilder中
		}

		return sb.toString();// 将StringBuilder转换为String返回
	}

}
```
由于是使用分隔符，所以"a,b,c"和"ab,c"在排序时是不同的，后面的'ab'将作为整体进行排序，这点需要注意。

## 2. 统计字符串中每个字符出现的次数

输入：           字符串


输出：           每个字符出现的次数


样例输入：       aabceedd


样例输出：       a=2,b=1,c=1,d=2,e=2



```java
package com.milanuo;

import java.util.Map;
import java.util.Map.Entry;
import java.util.Scanner;
import java.util.Set;
import java.util.TreeMap;

/**
 * @author 密拉诺
 */
public class Norepart4 {

	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("输入任意字符串：");
		String input = sc.nextLine();
		System.out.println(noRepeat(input));
		sc.close();
	}

	public static String noRepeat(String str) {
		char[] input = str.toCharArray();// 将输入的字符串转换为char数组

		Map<Character, Integer> map = new TreeMap<>();// 新建一个TreeMap
		for (int i = 0; i < input.length; i++) {
			if (map.containsKey(input[i])) {// 判断map中是否有key，有就把value+1
				Integer newNum = map.get(input[i]);
				map.put(input[i], newNum + 1);
			} else {
				map.put(input[i], 1);// 没有key，就把value设为1
			}
		}

		Set<Entry<Character, Integer>> entrySet = map.entrySet();// 获取所有的键-值对
		StringBuilder sb = new StringBuilder(64);
		for (Entry<Character, Integer> entry : entrySet) {
			sb.append(entry);// 拼接每一个键-值对
			sb.append(",");// 在诶一个键-值对后拼接","
		}
		sb.deleteCharAt(sb.length() - 1);// 删掉最后一个","号
		return sb.toString();// 将StringBuilder转换为String返回
	}

}
```
如果需要保留输入的顺序，只需将上面的TreeMap改为LinkedHashMap就可以了。


