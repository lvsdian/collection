[TOC]



### 概述

1. `ArrayList`是可以动态增长和缩减的索引序列，是基于数组实现的`List`类。

2. 该类封装了一个`Object[]`数组，每个类对象都有一个`capacity`属性，表示它们所封装的`object []`数组的长度，当向`ArrayList`中添加元素时，该属性值会自动增加。

   如果向`ArrayList`中添加大量元素，可以使用`ensureCapacity`方法一次性增加`capacity`，可以减少重新分配的次数提高性能。

3. `ArrayList`的用法和`Vector`向量类似，但是`Vector`是一个较老的集合，具有很多缺点，不建议使用，两者区别是：`ArrayList`线程不安全，当多条线程访问同一个`ArrayList`集合时，程序需要手动保证集合的同步性，而`Vector`是线程安全的，但性能低。

4. 结构图：

   <div align="center"> <img src="img/ArrayList继承关系.jpg"/> </div><br>



### 数据结构

​	底层数据结构就是数组，元素类型为`Object`类型，可以存放所有类型数据。

### 源码分析

#### 继承关系

 1. 继承`AbstractList`,`AbstractList`先实现`List<E>`，而不是让`ArrayList`直接实现`List<E>`：

    接口中全都是抽象的方法，而抽象类中可以有抽象方法，还可以有具体的实现方法，正是利用了这一点，让`AbstractList`实现接口中一些通用的方法，而具体的类如`ArrayList`就继承这个`AbstractList`类，拿到一些通用的方法，然后自己在实现一些自己特有的方法，这样一来，让代码更简洁，把继承结构最底层的类中通用的方法都抽取出来，先一起实现了，减少重复代码。

2. 实现`List<E>`接口：父类`AbstractList`已经实现了这个接口，子类可以不实现，这其实是一个mistake，因为他写这代码的时候觉得这个会有用处，但是其实并没什么用，但因为没什么影响，就一直留到了现在。

   实现`RandomAccess`接口：这个是一个标记性接口，它的作用就是用来快速随机存取，有关效率的问题，在实现了该接口的话，那么使用普通的for循环来遍历，性能更高，例如`ArrayList`。而没有实现该接口的话，使用Iterator来迭代，这样性能更高，例如`LinkedList`。所以这个标记性只是为了让我们知道我们用什么样的方式去获取数据性能更好。

3. `Cloneable`接口：实现了该接口，就可以使用`Object.Clone()`方法了。

4. `Serializable`接口：实现该序列化接口，表明该类可以被序列化，即能够从类变成字节流传输，然后还能从字节流变成原来的类。

#### 属性

```java
	/** 版本号 */
	private static final long serialVersionUID = 8683452581122892189L;

    /** 默认容量 */
    private static final int DEFAULT_CAPACITY = 10;

    /** 用无参构造的共享空数组实例。 */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
     * 用于无参构造的实例大小共享空的数组实例。
     * 我们将其与 EMPTY_ELEMENTDATA 区分开来，以了解添加第一个元素时应该膨胀多少。
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    /**
     * 存储元素的数组缓冲区
     * 这个缓冲区数组的长度就是ArrayList的容量 
     * Any empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
     * will be expanded to DEFAULT_CAPACITY when the first element is added.
     */
    transient Object[] elementData; // non-private to simplify nested class access

    /**
     * ArrayList的大小(the number of elements it contains).
     * @serial
     */
    private int size;
    /**
     * 数组最大可分配的大小。
     * Some VMs reserve(保留) some header words in an array.
     * Attempts to allocate larger arrays may result in
     * OutOfMemoryError: Requested array size exceeds(超过) VM limit
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
```

#### 构造方法

```java
    /** 指定初始化容量构造一个空的list */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }

    /** 初始容量为10的构造 */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * 构造一个包含指定集合的元素的list
     * 返回的顺序与这个集合的迭代器返回的顺序一致
	 */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }

```

