---
title: IoC
author: 高昊
avatar: 'https://wx1.sinaimg.cn/large/006bYVyvgy1ftand2qurdj303c03cdfv.jpg'
authorLink: 'https://github.com/spiritlord'
authorAbout: '技术宅,LOL'
authorDesc: '如果你也喜欢研究技术或者打LOL,那请来找我哦~'
categories: 技术
comments: true
date: 2020-04-07 17:15:38
tags:
keywords:
description:
photos:
---
# <center>**IoC**</center>

## 1.什么是IoC？
	反转控制是一种编程的风格或原则，可以解耦我们的一些相关设置。
	
## 2.IoC主要实现策略
	依赖查找和依赖注入。
	
## 3.IoC容器的职责
官方：
	1.实现与执行的任务之间，要产生解耦。
	2.要关注于这个模块，就这个任务上模块它所赋予设计的一个目的。（要关注于你的设计上的最终的目标，而不是它具体的实现）
	3.要释放这个模块，让其它系统知道它怎么样去运作，但是不是依赖于某个契约。
	4.当模块取消的时候，它的一个边缘的效应或者我们的副作用（主要为了解决这个问题）。
	5.好莱坞资源：不要来找我们（资源），我们会去找你
	
通用职责：
	依赖处理：
		依赖查找
		依赖注入
	生命周期管理：
		容器
		托管的资源（Java Beans或其他资源）
	配置：
		容器
		外部化配置
		托管的资源（Java Beans或其他资源）
		
## 4.IoC容器的实现

### 主要实现
Java SE:
	Java Beans
	Java ServiceLoader SPI
	JNDI (Java Naming and Directory Interface)
Java EE:
	EJB (Enterprise Java Beans)
	Servlet
开源:
	Apache Avalon (http://avalon.apache.org/closed.html)
	PicoContainer (http://picoContainer.com/)
	Google Guice (https://github.com/google/guice)
	Spring Framework (https://spring.io/projects/spring-framework)






























