---
title: 代码的瘦身计划
author: 高昊
avatar: https://wx1.sinaimg.cn/large/006bYVyvgy1ftand2qurdj303c03cdfv.jpg
authorLink: https://github.com/spiritlord
authorAbout: '技术宅,LOL'
authorDesc: '如果你也喜欢研究技术或者打LOL,那请来找我哦~'
categories: 技术
comments: true
date: 2019-10-28 18:00:07
tags:
 - jdk1.8
 - lambda
keywords: lambda
description: JDK1.8技术分享
photos: https://static.2heng.xin/wp-content/uploads//2019/02/wallhaven-672007-1-1024x576.png
---
# <center>**Lambda表达式**</center>
　　Java由于跨平台特性使之一直处于编程解老大的地位，特别自微服务面世之后更是长期居于编程界第一名的地位。但随着Python、Scala等函数式编程语言的崛起，Java的霸主地位得到了挑战。特别是随着大数据和人工智能的崛起，Python更是超越了Java位居第一榜位。
　　不过从JDK1.8以后，Java语言发生了巨大的改变（Lambda表达式、函数式接口、方法引用和构造器调用、Stream API、接口中的默认方法和静态方法、新时间日期API....）。
	我们今天从代码优化的角度分享一下Lambda表达式。

## lambda简介
`Lambda表达式本质上是一段匿名内部类，也可以是一段可以传递的代码。`

## 代码比对＆优化

### 数组
`如果要对数组类型用lambda表达式的话，可以通过如下代码转化为stream流再进行操作，如：`
```java
String[] words = {"one", "two", "three", "four"};
Stream<String> stream = Arrays.stream(words);
```

### 遍历list
```java
List<String> words = Arrays.asList(new String[]{"one", "two", "three", "four"});
// jdk1.8之前
for (int i = 0; i < words.size(); i++) {
	System.out.print(words.get(i) + " ");
}
// 或
for (String word : words) {
	System.out.print(word + " ");
}
// jdk1.8 Lambda表达式
words.forEach(word -> System.out.print(word + " "));
// forEach循环如果只做输出操作的话，可以简写为：
words.forEach(System.out::print);
```
`输出结果：1 2 3 4 5`

### list值求和
```java
List<Integer> nums = Arrays.asList(new Integer[]{1, 2, 3, 4, 5});
// jdk1.8之前
int count = 0;
for (int i = 0; i < nums.size(); i++) {
	count += nums.get(i);
}
// 或
for (int num : nums) {
	count += num;
}
// jdk1.8 Lambda表达式
count = nums.stream().reduce(0, (a, b) -> a + b);
System.out.println(count);
```
`输出结果：15`

### 统计list集合中某元素出现的个数
```java
// 统计words集合中<one>元素出现的个数
List<String> words =
		Arrays.asList(new String[]{"one", "two", "three", "four", "one", "one"});
// jdk1.8之前
long num = 0;
for (int i = 0; i < words.size(); i++) {
	if ("one".equals(words.get(i))) {
		num++;
	}
}
// 或
for (String word : words) {
	if ("one".equals(word)) {
		num++;
	}
}
// jdk1.8 Lambda表达式
num = words.stream().filter(x -> x.equals("one")).count();
System.out.println(num);
```
`输出结果：3`

### list集合去重
```java
// jdk1.8之前(因为list去重的方法有很多，这里只写一个常用的)
List<String> words =
		Arrays.asList(new String[]{"one", "two", "three", "four", "one", "one"});
List<String> wordsNew = new ArrayList<>();
for (String word : words) {
	if (!wordsNew.contains(word)) {
		wordsNew.add(word);
	}
}
// jdk1.8 Lambda表达式
words = words.stream().distinct().collect(toList());
words.forEach(System.out::println);
```
`输出结果：one two three four`

### list集合对象去重
```java
@Data
@AllArgsConstructor
public class User {

    /**
     * id
     */
    private String id;

    /**
     * 姓名
     */
    private String name;

    /**
     * 年龄
     */
    private String age;
}

// jdk1.8之前
/*
 *  这里稍作复杂就不做过多赘述，且没太大意义
 */
 
// jdk1.8 Lambda表达式
List<User> users = Arrays.asList(
		new User("1", "Davis", "23"),
		new User("2", "Davis", "20"),
		new User("3", "Garcia", "22"),
		new User("4", "Miller", "23"));
// 根据姓名去重
users = users.stream().collect(
		Collectors.collectingAndThen(
				Collectors.toCollection(
						() -> new TreeSet<>(
								Comparator.comparing(User::getName))),
						ArrayList::new));
users.forEach(System.out::println);
```
`输出结果：`
```java
User(id=1, name=Davis, age=23)
User(id=3, name=Garcia, age=22)
User(id=4, name=Miller, age=23)
```

### 遍历map
```java
Map<String, Object> userMap = new LinkedHashMap<>(16);
userMap.put("id", "1");
userMap.put("name", "Miller");
userMap.put("age", "23");
// jdk1.8之前(这里只写一个常用的)
for (Map.Entry<String, Object> entry : userMap.entrySet()) {
	System.out.println("key：" + entry.getKey() + ",value：" + entry.getValue());
}
// jdk1.8 Lambda表达式
userMap.forEach((k, v) -> System.out.println("key：" + k + ",value：" + v));
```
`输出结果：`
```java
key：id,value：1
key：name,value：Miller
key：age,value：23
```

## 还有啥，一时想不起来......

To be continued...