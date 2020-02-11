* [快速失败机制(fail\-fast)](#%E5%BF%AB%E9%80%9F%E5%A4%B1%E8%B4%A5%E6%9C%BA%E5%88%B6fail-fast)
* [安全失败机制(fail\-safe)](#%E5%AE%89%E5%85%A8%E5%A4%B1%E8%B4%A5%E6%9C%BA%E5%88%B6fail-safe)
* [与HashMap区别](#%E4%B8%8Ehashmap%E5%8C%BA%E5%88%AB)

### 快速失败机制(`fail-fast`)

- 使用迭代器遍历一个集合对象时，如果边遍历边修改集合，就会抛出`Concurrent Modification Exception`，因为遍历过程中维护了一个修改次数的变量(`modCount`)，每次遍历下一个元素都会检测`modCount`是否等于`expectedmodCount`，如果不等就会抛异常。
- 如果边遍历边用迭代器来修改集合不会报异常。

### 安全失败机制(`fail-safe`)

- 使用安全失败的集合容器在遍历时不是在原容器上遍历，而是先复制原容器，在拷贝的容器上遍历。
- 由于迭代时是在拷贝容器上进行遍历，所以对原容器的修改不会被检测到，所以不会抛异常。

### 与HashMap区别

- `hashMap`线程不安全，`hashTable`线程安全，但是通过加`synchronized`，所以效率不高。

- `hashMap`键值都可以为`null`，`hashTable`键值不能为`null`。

  - `hashTable`的`put()`方法在`key`为`null`时直接抛`NullPointerException`。
  - `hashTable`使用的安全失败机制，因此每次读到的数据不一定是最新的。如果使用`null`就无法判断是`key`为`null`还是`key`不存在。`ConcurrentHashMap`同理。

- `hashTable`初始容量为`11`，`hashMap`初始容量为`16`。负载因子默认都是`0.75`。

- `hashTable`扩容后长度为`原来长度翻倍 +  1`，`hashMap`扩容后长度为`原来长度翻倍 `

- `HashMap`中的`Iterator`迭代器是`fail-fast`的，而`HastTable`的`Enumerator`不是`fail-fast`的。

  

