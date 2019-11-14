---
title: 逻辑艺术1
author: 高昊
avatar: 'https://wx1.sinaimg.cn/large/006bYVyvgy1ftand2qurdj303c03cdfv.jpg'
authorLink: 'https://github.com/spiritlord'
authorAbout: '技术宅,LOL'
authorDesc: '如果你也喜欢研究技术或者打LOL,那请来找我哦~'
categories: 技术
comments: true
date: 2019-10-31 11:02:19
tags:
 - 算法
 - 数据结构
keywords: 数据结构
description: 数组也有大用处哟~
photos: https://static.2heng.xin/wp-content/uploads//2019/02/wallhaven-672007-1-1024x576.png
---
# <center>**数组的妙用**</center>
　　从学习了集合类以后，我们对数组的运用就很少了，甚至是就没怎么用过数组了。我们也知道数组有很多的局限性，比如查找特定数据速度慢、一经创建便不能改变大小、只能存储一种类型的数据、剩余未使用的空间数据不安全等。
　　那我们能不能从数据结构的角度出发，对数组进行优化呢？今天，我来跟大家一起编写一个动态数组，解决它一部分的局限性。

## 数组的缺点
- 查找特定数据速度慢
- 数组大小必须创建时给出
- 一经创建便不能改变大小
- 只能存储一种类型的数据
- 剩余未使用的空间数据不安全
- 数组的空间必须是连续的，可能造成内存浪费

## 创建一个属于自己的动态数组

### 1. 数组的创建
`我们利用Java原有的静态数组创建一个动态数组，并重写toString()方法`
```java
/**
 * 在类的后面加上<E>是为了使我们的代码变得灵活起来
 */
public class DynamicArray<E> {

	/**
     * 数据存放
     */
	private E[] data;

	/**
     * 有效元素数
     */
    private int size;
	
	public DynamicArray(int capacity) {
        data = (E[]) new Object[capacity];
        size = 0;
    }
	
	/**
     * 如果调用无参构造，默认容量为10
     */
	public DynamicArray() {
        this(10);
    }
	
	public int getSize() {
        return size;
    }
	
	public int getCapacity() {
        return data.length;
    }
	
	public boolean isEmpty() {
        return size == 0;
    }
	
	@Override
    public String toString() {
        StringJoiner res = new StringJoiner("");
        res.add(String.format("数组：size = %d, capacity = %d\n", size, data.length));
        res.add("[");
        for (int i = 0; i < size; i++) {
            res.add(String.valueOf(data[i]));
            if (i != size - 1) {
                res.add(",");
            }
        }
        res.add("]");
        return res.toString();
    }
}
```
创建数组时，如果规定了泛型的类型，那么只能创建一个特定类型的数组。如果不规定泛型的类型，当你传值时jvm会帮你自动判断数据的类型，而且这个时候数组就实现了可以`存储多个类型的元素`。

### 2. 为动态数组添加(增、查、改、删)

#### 2.1. 增
```java
/**
 * 在第index的位置插入一个新数据e
 * @param index 索引位置
 * @param e		插入数据
 */
public void add(int index, E e) {
	if (size == data.length) {
		throw new IllegalArgumentException("数组已满");
	}
	if (index < 0 || index > size) {
		throw new IllegalArgumentException("数组下标非法");
	}
	for (int i = size - 1; i >= index; i--) {
		data[i + 1] = data[i];
	}
	data[index] = e;
	size++;
    }
```
首先判断传入数据的合法性，如果数组已满则抛出异常。该方法支持在已存入连续数据之间插入新数据，如果新插入数据传入的索引位置大于数组最后一个数据的索引位置，导致存放数据不连续则抛出异常。
根据这个方法可以再延伸出两个方法：
```java
/**
 * 在所有数据后添加一个新数据
 * @param e 插入数据
 */
public void addLast(E e) {
	add(size, e);
}
/**
 * 在所有数据前添加一个新数据
 * @param e 插入数据
 */
 public void addFirst(E e) {
	add(0, e);
}
```

#### 2.2. 查
```java
/**
 * 根据索引查询数据
 * @param index 索引位置
 * @return
 */
public E get(int index) {
	if (index < 0 || index >= size) {
		throw new IllegalArgumentException("数组下标非法");
	}
	return data[index];
}
/**
 * 查找数组中是否包含数据
 * @param e 查找数据
 * @return
 */
 public boolean contains(E e) {
	for (int i = 0; i < size; i++) {
		if (data[i].equals(e)) {
			return true;
		}
	}
	return false;
}
/**
 * 查找数组中元素e所在的索引,如果不存在,则返回-1
 * @param e 查找数据
 * @return
 */
 public int findIndex(E e) {
	for (int i = 0; i < size; i++) {
		if (data[i].equals(e)) {
			return i;
		}
	}
	return -1;
}
```

#### 2.3. 改
```java
/**
 * 根据索引修改数据
 * @param index 索引位置
 * @param e		修改数据
 */
 public void set(int index, E e) {
	if (index < 0 || index >= size) {
		throw new IllegalArgumentException("数组下标非法");
	}
	data[index] = e;
}
```

#### 2.4. 删
```java
/**
 * 根据索引删除数据,并返回删除的数据
 * @param index 索引位置
 * @return
 */
 public E remove(int index) {
	if (index < 0 || index >= size) {
		throw new IllegalArgumentException("数组下标非法");
	}
	E ret = data[index];
	for (int i = index + 1; i < size; i++) {
		data[i - 1] = data[i];
	}
	size--;
	data[size] = null;
	return ret;
}
```
跟添加数据一样的道理，我们可以为删除延伸出三个方法：
```java
/**
 * 删除第一个数据,并返回删除的数据
 * @return
 */
 public E removeFirst() {
	return remove(0);
}
 /**
 * 删除最后一个数据,并返回删除的数据
 * @return
 */
 public E removeLast() {
	return remove(size - 1);
}
/**
 * 删除指定数据
 * @param e 删除数据
 * @return
 */
 public boolean removeElement(E e) {
	int index = findIndex(e);
	if (index != -1) {
		remove(index);
		return true;
	}
	return false;
}
```

### 2. 使数组变成动态数组
数组的大小一经创建则不能改变，可我们创建数组的时候大小设置为多少比较合适呢？如果数组满了我还想继续添加怎么办？数组存放数据量太少，剩余过多空间造成内存浪费怎么办？
`通过数组的扩容和缩容来达到动态数组的目的，完美的解决了上述问题。`
```java
/**
 * 数组扩容或缩容
 * @param newCapacity 容量
 */
 private void resize(int newCapacity) {
	E[] newData = (E[]) new Object[newCapacity];
	for (int i = 0; i < size; i++) {
		newData[i] = data[i];
	}
	data = newData;
}
```
`上述代码通过创建一个新的数组存放之前数组的数据，然后把旧数组的引用指向新数组来完成数组的扩容或缩容。`
我们还需要在添加数据代码add()和删除数据代码remove()中分别加入以下的判断：
```java
// 添加数据时，如果数组已满则扩容
if (size == data.length) {
	resize(data.length * 2);
}
// 删除数据时，如果剩余空间过多则缩容
if (size == data.length / 4 && data.length / 2 != 0) {
	resize(data.length / 2);
}
```

## 代码整合
`我们把部分代码优化一下，并把重复的代码提取出来，全部代码如下(不包含注释)：`
```java
public class DynamicArray<E> {

    private E[] data;

    private int size;

    public DynamicArray(int capacity) {
        data = (E[]) new Object[capacity];
        size = 0;
    }

    public DynamicArray() {
        this(10);
    }

    public int getSize() {
        return size;
    }

    public int getCapacity() {
        return data.length;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public void addLast(E e) {
        add(size, e);
    }

    public void addFirst(E e) {
        add(0, e);
    }

    public void add(int index, E e) {
        if (index < 0 || index > size) {
            throw new IllegalArgumentException("数组下标非法");
        }
        if (size == data.length) {
            resize(data.length * 2);
        }
        for (int i = size - 1; i >= index; i--) {
            data[i + 1] = data[i];
        }
        data[index] = e;
        size++;
    }

    public E get(int index) {
        verify(index);
        return data[index];
    }

    public void set(int index, E e) {
        verify(index);
        data[index] = e;
    }

    public boolean contains(E e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e)) {
                return true;
            }
        }
        return false;
    }

    public int findIndex(E e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e)) {
                return i;
            }
        }
        return -1;
    }

    public E remove(int index) {
        verify(index);
        E ret = data[index];
        for (int i = index + 1; i < size; i++) {
            data[i - 1] = data[i];
        }
        size--;
        data[size] = null;
        if (size == data.length / 4 && data.length / 2 != 0) {
            resize(data.length / 2);
        }
        return ret;
    }

    public E removeFirst() {
        return remove(0);
    }

    public E removeLast() {
        return remove(size - 1);
    }

    public boolean removeElement(E e) {
        int index = findIndex(e);
        if (index != -1) {
            remove(index);
            return true;
        }
        return false;
    }

    @Override
    public String toString() {
        StringJoiner res = new StringJoiner("");
        res.add(String.format("数组：size = %d, capacity = %d\n", size, data.length));
        res.add("[");
        for (int i = 0; i < size; i++) {
            res.add(String.valueOf(data[i]));
            if (i != size - 1) {
                res.add(",");
            }
        }
        res.add("]");
        return res.toString();
    }

    private void verify(int index) {
        if (index < 0 || index >= size) {
            throw new IllegalArgumentException("数组下标非法");
        }
    }

    private void resize(int newCapacity) {
        E[] newData = (E[]) new Object[newCapacity];
        for (int i = 0; i < size; i++) {
            newData[i] = data[i];
        }
        data = newData;
    }
}
```

## FAQ
为什么数组进行缩容的时候，要进行`size == data.length / 4 && data.length / 2 != 0`这样的判断呢？
有机会再为大家解读哦，什么是数组的`复杂度震荡`。

## 还有啥，一时想不起来......

To be continued...