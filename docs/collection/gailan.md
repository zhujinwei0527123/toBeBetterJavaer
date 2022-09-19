---
title: Java 集合框架（容器）体系结构
shortTitle: Java集合框架体系结构
category:
  - Java核心
tag:
  - 集合框架（容器）
description: Java程序员进阶之路，小白的零基础Java教程，Java 集合框架（容器）体系结构
head:
  - - meta
    - name: keywords
      content: Java,Java SE,Java基础,Java教程,Java程序员进阶之路,Java入门,教程,集合框架,容器
---


眼瞅着三妹的王者荣耀杀得正嗨，我趁机喊到：“别打了，三妹，我们来一起学习 Java 的集合框架吧。”

“才不要呢，等我打完这一局啊。”三妹倔强地说。

“好吧。”我只好摊摊手地说，“那我先画张集合框架的结构图等着你。”

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/collection/gailan-01.png)


“完了没？三妹。”

“完了好一会儿了，二哥，你图画得真慢，让我瞧瞧怎么样？”

“害，图要画得清晰明了，不容易的。三妹，你瞧，不错吧。”

Java 集合框架可以分为两条大的支线：

- Collection，主要由 List、Set、Queue 组成，List 代表有序、可重复的集合，典型代表就是封装了动态数组的 ArrayList 和封装了链表的 LinkedList；Set 代表无序、不可重复的集合，典型代表就是 HashSet 和 TreeSet；Queue 代表队列，典型代表就是双端队列 ArrayDeque，以及优先级队列 PriorityQue。
- Map，代表键值对的集合，典型代表就是 HashMap。

“接下来，我们再来过一遍。”

## 01、List

>List 的特点是存取有序，可以存放重复的元素，可以用下标对元素进行操作

### **1）ArrayList**

- ArrayList 是由数组实现的，支持随机存取，也就是可以通过下标直接存取元素；
- 从尾部插入和删除元素会比较快捷，从中间插入和删除元素会比较低效，因为涉及到数组元素的复制和移动；
- 如果内部数组的容量不足时会自动扩容，因此当元素非常庞大的时候，效率会比较低。

### **2）LinkedList**

- LinkedList 是由双向链表实现的，不支持随机存取，只能从一端开始遍历，直到找到需要的元素后返回；
- 任意位置插入和删除元素都很方便，因为只需要改变前一个节点和后一个节点的引用即可，不像 ArrayList 那样需要复制和移动数组元素；
- 因为每个元素都存储了前一个和后一个节点的引用，所以相对来说，占用的内存空间会比 ArrayList 多一些。

### **3）Vector 和 Stack**

List 的实现类还有一个 Vector，是一个元老级的类，比 ArrayList 出现得更早。ArrayList 和 Vector 非常相似，只不过 Vector 是线程安全的，像 get、set、add 这些方法都加了 `synchronized` 关键字，就导致执行执行效率会比较低，所以现在已经很少用了。

更好的选择是并发包下的 CopyOnWriteArrayList。

Stack 是 Vector 的一个子类，本质上也是由动态数组实现的，只不过还实现了先进后出的功能（在 get、set、add 方法的基础上追加了 pop、peek 等方法），所以叫栈。

不过，由于 Stack 执行效率比较低（方法上同样加了 synchronized 关键字），就被双端队列 ArrayDeque 取代了。

## 02、Set

> Set 的特点是存取无序，不可以存放重复的元素，不可以用下标对元素进行操作，和 List 有很多不同

## **1）HashSet**

HashSet 其实是由 HashMap 实现的，只不过值由一个固定的 Object 对象填充，而键用于操作。

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
    private transient HashMap<E,Object> map;

    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();

    public HashSet() {
        map = new HashMap<>();
    }

    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }

    public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }
}
```

### **2）LinkedHashSet**

LinkedHashSet 继承自 HashSet，其实是由 LinkedHashMap 实现的，LinkedHashSet 的构造方法调用了 HashSet 的一个特殊的构造方法：

```java
HashSet(int initialCapacity, float loadFactor, boolean dummy) {
   map = new LinkedHashMap<>(initialCapacity, loadFactor);
}
```

### **3）TreeSet**

“二哥，不用你讲了，我能猜到，TreeSet 是由 TreeMap 实现的，只不过同样操作的键位，值由一个固定的 Object 对象填充。”

哇，三妹都学会了推理。

“是的，总体上来说，Set 集合不是关注的重点，因为底层都是由 Map 实现的，为什么要用 Map 实现呢？三妹你能猜到原因吗？”

“让我想想。”

“嗯？难道是因为 Map 的键不允许重复、无序吗？”

老天，竟然被三妹猜到了。

“是的，你这水平长进了呀，三妹。”

## 03、Queue

> Queue，也就是队列，通常遵循先进先出（FIFO）的原则，新元素插入到队列的尾部，访问元素返回队列的头部。

### **1）ArrayDeque**

从名字上可以看得出，ArrayDeque 是一个基于数组实现的双端队列，为了满足可以同时在数组两端插入或删除元素的需求，数组必须是循环的，也就是说数组的任何一点都可以被看作是起点或者终点。

这是一个包含了 4 个元素的双端队列，和一个包含了 5 个元素的双端队列。

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/collection/gailan-02.png)

head 指向队首的第一个有效的元素，tail 指向队尾第一个可以插入元素的空位，因为是循环数组，所以 head 不一定从是从 0 开始，tail 也不一定总是比 head 大。

### **2）LinkedList**

LinkedList 一般都归在 List 下，只不过，它也实现了 Deque 接口，可以作为队列来使用。等于说，LinkedList 同时实现了 Stack、Queue、PriorityQueue 的所有功能。

### **3）PriorityQueue**

PriorityQueue 是一种优先级队列，它的出队顺序与元素的优先级有关，执行 remove 或者 poll 方法，返回的总是优先级最高的元素。

要想有优先级，元素就需要实现 Comparable 接口或者 Comparator 接口。

## 04、Map

> Map 保存的是键值对，键要求保持唯一性，值可以重复。

### **1）HashMap**

HashMap 实现了 Map 接口，根据键的 HashCode 值来存储数据，具有很快的访问速度，最多允许一个 null 键。

HashMap 不论是在学习还是工作当中，使用频率都是相当高的。随着 JDK 版本的不断更新，HashMap 的底层也优化了很多次，JDK 8 的时候引入了红黑树。

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    HashMap.Node<K,V>[] tab; HashMap.Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        HashMap.Node<K,V> e; K k;
        if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof HashMap.TreeNode)
            e = ((HashMap.TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
    return null;
}
```

一旦 HashMap 发生哈希冲突，就把相同键位的地方改成链表，如果链表的长度超过 8，就该用红黑树。

### **2）LinkedHashMap**

大多数情况下，只要不涉及线程安全问题，Map基本都可以使用HashMap，不过HashMap有一个问题，就是迭代HashMap的顺序并不是HashMap放置的顺序，也就是无序。HashMap的这一缺点往往会带来困扰，因为有些场景，我们期待一个有序的Map。

于是 LinkedHashMap 就闪亮登场了。LinkedHashMap 是 HashMap 的子类，内部使用链表来记录插入/访问元素的顺序。

LinkedHashMap 可以看作是 HashMap + LinkedList 的合体，它使用了 哈希表来存储数据，又用了双向链表来维持顺序。

### **3）TreeMap**

HashMap 是无序的，所以遍历的时候元素的顺序也是不可测的。TreeMap 是有序的，它在内部会对键进行排序，所以遍历的时候就可以得到预期的顺序。

为了保证顺序，TreeMap 的键必须要实现 Comparable 接口或者 Comparator 接口。

----

最近整理了一份牛逼的学习资料，包括但不限于Java基础部分（JVM、Java集合框架、多线程），还囊括了 **数据库、计算机网络、算法与数据结构、设计模式、框架类Spring、Netty、微服务（Dubbo，消息队列） 网关** 等等等等……详情戳：[可以说是2022年全网最全的学习和找工作的PDF资源了](https://tobebetterjavaer.com/pdf/programmer-111.html)

关注二哥的原创公众号 **沉默王二**，回复**111** 即可免费领取。

![](http://cdn.tobebetterjavaer.com/tobebetterjavaer/images/xingbiaogongzhonghao.png)
