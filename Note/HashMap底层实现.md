# HashMap底层实现

[TOC]

## 概述

1. HashMap 是基于哈希表的 map 接口的非同步实现,允许 null 值,不保证顺序
2. 归纳: HashMap 在底层将 key-value 当成一个 Entry 对象整体进行存储.当需要存储一个 Entry 对象时，会根据hash算法来决定其在数组中的存储位置，在根据equals方法决定其在该数组位置上的链表中的存储位置；当需要取出一个Entry时，也会根据hash算法找到其在数组中的存储位置，再根据equals方法从该位置上的链表中取出该Entry。

## 数据结构

1. 数组和链表的结合体

## Hash算法

​	计算保存的索引

```
static int indexFor(int h, int length) {
    return h & (length-1);
}
```

​	保证数组长度总是2的 n 次方

```
int capacity = 1;
    while (capacity < initialCapacity)
        capacity <<= 1;
```

​	**当length总是 2 的n次方时，h& (length-1)运算等价于对length取模，也就是h%length，但是&比%具有更高的效率。**

​	put 方法:

​	当程序试图将一个key-value对放入HashMap中时，程序首先根据该 key的 hashCode() 返回值决定该 Entry 的存储位置：如果两个 Entry 的 key 的 hashCode() 返回值相同，那它们的存储位置相同。如果这两个 Entry 的 key 通过 equals 比较返回 true，新添加 Entry 的 value 将覆盖集合中原有Entry 的 value，但key不会覆盖。如果这两个 Entry 的 key 通过 equals 比较返回 false，新添加的 Entry 将与集合中原有 Entry 形成 Entry 链，而且新添加的 Entry 位于 Entry 链的头部——具体说明继续看 addEntry() 方法的说明。

## resize

​	当需要对 HashMap 进行扩容的时候,**最消耗性能的点就出现了,原数组中的数据必须重新计算其在新数组中的位置，并放进去，这就是resize**

​	所以如果我们已经预知HashMap中元素的个数，那么**预设元素的个数**能够有效的提高HashMap的性能

## Fail fast 机制

​	我们知道java.util.HashMap不是线程安全的，在迭代器创建之后，如果从结构上对映射进行修改，除非通过迭代器本身的 remove 方法，其他任何时间任何方式的修改，迭代器都将抛出**ConcurrentModificationException**，这就是所谓fail-fast策略

​	这一策略在源码中的实现是通过modCount域，**modCount顾名思义就是修改次数**，对HashMap内容的修改都将增加这个值，那么在迭代器初始化过程中会将这个值赋给迭代器的expectedModCount,在迭代过程中，判断modCount跟expectedModCount是否相等，如果不相等就表示已经有其他线程修改了Map

## 参考

http://www.cnblogs.com/xwdreamer/archive/2012/06/03/2532832.html

http://www.open-open.com/lib/view/open1411442233765.html

