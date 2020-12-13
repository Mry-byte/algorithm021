基于Hash table 实现 Map 接口映射，允许 key 与 value 为 null 值。在 Java 1.7 版本中是用数组+链表的数据结构实现的，1.8之后使用数组+链表+红黑树的数据结构实现。

HashMap 是非同步的，也就是说是线程不安全的，通常是通过在自然封装HashMap的某个对象上同步来实现同步的。如果不存在这样的对象，则应该使用集合“包装”HashMap，synchronizedMap方法。

```plain
Map m = Collections.synchronizedMap(new HashMap(...));
```
#### 数据结构

![图片](https://uploader.shimo.im/f/EmIg2mjkOX0VD3It.png!thumbnail?fileGuid=RXVpdYJTGwQRjtkQ)


---


图中数组，也就是bucket（桶），数组的长度，也就是桶的数量：

```plain
/**
 * The table, initialized on first use, and resized as
 * necessary. When allocated, length is always a power of two.
 * (We also tolerate length zero in some operations to allow
 * bootstrapping mechanics that are currently not needed.)
 */
transient Node<K,V>[] table;
```

---
#### 

数组中存放的链表节点，HashMap中是以 Node 节点数组类型存放数据的。当

```plain
/**
 * Basic hash bin node, used for most entries.  (See below for
 * TreeNode subclass, and in LinkedHashMap for its Entry subclass.)
 */
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;
    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }
    public final K getKey()        { return key; }
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }
    public final int hashCode() {
        return Objects.hashCode(key) ^ Objects.hashCode(value);
    }
    public final V setValue(V newValue) {
        V oldValue = value;
        value = newValue;
        return oldValue;
    }
    public final boolean equals(Object o) {
        if (o == this)
            return true;
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;
            if (Objects.equals(key, e.getKey()) &&
                Objects.equals(value, e.getValue()))
                return true;
        }
        return false;
    }
}
```

---
#### 

当链表中的元素超过了8个以后，会将链表转换为红黑树，其实最终继承的还是HashMap.Node<K,V>，因为1.7采用的是数组+链表方式的数据结构，hash冲突后是用单链表进行的纵向延伸，使用头插法来提高插入的效率，但是容易并发出现逆序且环形链表死循环的问题，1.8之后采用红黑树尾插法，能够避免出现此问题。

当红黑树中节点数小于 6 个时又会转为单链表来存储数据。

```plain
/**
 * Entry for Tree bins. Extends LinkedHashMap.Entry (which in turn
 * extends Node) so can be used as extension of either regular or
 * linked node.
 */
static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
	TreeNode<K,V> parent;  // red-black tree links
	TreeNode<K,V> left;
	TreeNode<K,V> right;
	TreeNode<K,V> prev;    // needed to unlink next upon deletion
	boolean red;
	TreeNode(int hash, K key, V val, Node<K,V> next) {
		super(hash, key, val, next);
	}
    /**
     * Returns root of tree containing this node.
     */
    final TreeNode<K,V> root() {
        for (TreeNode<K,V> r = this, p;;) {
            if ((p = r.parent) == null)
                return r;
            r = p;
        }
    }
    ...
}
```
#### 继承关系

```plain
Class HashMap<K,​V>
  java.lang.Object
    java.util.AbstractMap<K,​V>
      java.util.HashMap<K,​V>
Type Parameters:
K - the type of keys maintained by this map
V - the type of mapped values
All Implemented Interfaces:
Serializable, Cloneable, Map<K,​V>
Direct Known Subclasses:
LinkedHashMap, PrinterStateReasons
```
#### 构造方法

* HashMap() , Constructs an empty HashMap with the default initial capacity (16) and the default load factor (0.75).
* HashMap(int initialCapacity) , Constructs an empty HashMap with the specified initial capacity and the default load factor (0.75).
* HashMap(int initialCapacity , float loadFactor) , Constructs an empty HashMap with the specified initial capacity and load factor.
* HashMap(Map<? extend K , ? extends V )) , Constructs a new HashMap with the same mappings as the specified Map.
#### 两个影响性能的重要参数 initial capacity and load factor.

An instance of HashMap has two parameters that affect its performance: initial capacity and load factor. The capacity is the number of buckets in the hash table, and the initial capacity is simply the capacity at the time the hash table is created. The load factor is a measure of how full the hash table is allowed to get before its capacity is automatically increased. When the number of entries in the hash table exceeds the product of the load factor and the current capacity, the hash table is rehashed (that is, internal data structures are rebuilt) so that the hash table has approximately twice the number of buckets.

HashMap的实例具有两个影响其性能的参数：初始容量和负载因子。容量是哈希表中存储桶的数量，初始容量只是创建哈希表时的容量。负载因子是在自动增加其哈希表容量之前允许哈希表获得的满度的度量。当哈希表中的条目数超过负载因子和当前容量的乘积时，哈希表将被重新哈希（即，内部数据结构将被重建），因此哈希表的存储桶数约为两倍。

两个参数的默认值分别为：

```plain
/**
 * The default initial capacity - MUST be a power of two.
 */
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
/**
 * The load factor used when none specified in constructor.
 */
static final float DEFAULT_LOAD_FACTOR = 0.75f;
```
*问题一：为什么HashMap默认加载因子是0.75？*
答案：**提高空间利用率和 减少查询成本的折中，主要是泊松分布，0.75的话碰撞最小。**

参考博客：[https://www.cnblogs.com/aspirant/p/11470928.html](https://www.cnblogs.com/aspirant/p/11470928.html)

#### 常用方法列举

|**Modifier and Type**|**Method**|**Description**|
|:----|:----|:----|
|void|[clear](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#clear())()|Removes all of the mappings from this map.|
|[Object](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Object.html)|[clone](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#clone())()|Returns a shallow copy of this HashMap instance: the keys and values themselves are not cloned.|
|boolean|[containsKey](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#containsKey(java.lang.Object))​([Object](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Object.html)key)|Returns true if this map contains a mapping for the specified key.|
|boolean|[containsValue](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#containsValue(java.lang.Object))​([Object](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Object.html)value)|Returns true if this map maps one or more keys to the specified value.|
|[Set](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Set.html)<[Map.Entry](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Map.Entry.html)<[K](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html),​[V](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)>>|[entrySet](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#entrySet())()|Returns a[Set](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Set.html)view of the mappings contained in this map.|
|[V](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)|[get](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#get(java.lang.Object))​([Object](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Object.html)key)|Returns the value to which the specified key is mapped, or null if this map contains no mapping for the key.|
|boolean|[isEmpty](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#isEmpty())()|Returns true if this map contains no key-value mappings.|
|[Set](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Set.html)<[K](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)>|[keySet](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#keySet())()|Returns a[Set](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Set.html)view of the keys contained in this map.|
|[V](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)|[put](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#put(K,V))​([K](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)key,[V](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)value)|Associates the specified value with the specified key in this map.|
|void|[putAll](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#putAll(java.util.Map))​([Map](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/Map.html)<? extends[K](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html),​? extends[V](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)> m)|Copies all of the mappings from the specified map to this map.|
|[V](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html)|[remove](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#remove(java.lang.Object))​([Object](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/lang/Object.html)key)|Removes the mapping for the specified key from this map if present.|
|int|[size](https://docs.oracle.com/en/java/javase/12/docs/api/java.base/java/util/HashMap.html#size())()|Returns the number of key-value mappings in this map.|

#### 重要方法解析

```plain
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
               boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    // 如果为null，则扩容
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    // 如果tab对应的数组位置上的数据为null，则新建node，并指向它
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        // 比较hash值和key值都相等，说明插入的键值已经存在，给e节点赋值为p
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        // P 节点是TreeNode类型，执行树节点的插入操作     
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        // 遍历插入单链表中，采用尾插法    
        else {
            for (int binCount = 0; ; ++binCount) {
                // 尾插法，遍历到最后一个next节点为null的节点，将next节点赋值为新创建的节点
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    // 这里判断，当单链表节点数大于等于树化阈值，则转化为红黑树存储，-1是因为 binCount 下标是从 0 开始的。
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                // 在桶中找到了对应的key，赋值给e，退出循环
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                // 没有找到，则继续向下一个节点寻找
                p = e;
            }
        }
        // 根据onlyIfAbsent来判断是否需要修改旧值
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            //钩子函数，用于给LinkedHashMap继承后使用，在HashMap里是空的
            afterNodeAccess(e);
            return oldValue;
        }
    }
    // 修改计数器大小，+1
    ++modCount;
    // 实际条目数+1 ，如果大于阈值，需要重新计算并扩容
    if (++size > threshold)
        resize();
    //钩子函数，用于给LinkedHashMap继承后使用，在HashMap里是空的
    afterNodeInsertion(evict);
    return null;
} 
```

---


```plain
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        // 先检查数组下标第一个节点是否满足，如果满足，直接返回
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        // 数组第一个节点不满足，则循环遍历桶中元素
        if ((e = first.next) != null) {
            // 如果是树节点，则采用树节点的方式来获取对应key的值
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            // 否则就遍历单链表中元素，直到找到为止
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

---


```plain
final Node<K,V>[] resize() {
    // 记录当前数组
    Node<K,V>[] oldTab = table;
    // 记录当前数组长度
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    // 记录当前阈值
    int oldThr = threshold;
    // 使用变量newCap记录新数组的长度，newThr记录新阈值
    int newCap, newThr = 0;
    if (oldCap > 0) {
        //容量达到最大容量，则直接将阈值设置为最大值，并返回旧table
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        // 在旧table容量的基础上扩容两倍，但是扩容后的容量必须小于最大容量，并且在旧容量大于默认初始化容量时将旧阈值也扩大至旧阈值的两倍
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            newThr = oldThr << 1; // double threshold
    }
    // 当容量 = 0 && 阈值 > 0 时将容量设置为阈值大小
    else if (oldThr > 0) // initial capacity was placed in threshold
        newCap = oldThr;
        // 此情况应该是调用无参构造时的初始化操作
    else {               // zero initial threshold signifies using defaults
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    // 新阈值==0，将其赋值为 新容量*加载因子乘积大小且最大值为Integer.MAX_VALUE
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    // 将当前阈值修改为新阈值
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    // 将当前table修改为使用新容量创建的新table
    table = newTab;
    // 下面逻辑是为了将旧table中的数据迁移至新table中
    if (oldTab != null) {
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            // 将旧table中元素赋值给临时变量 e 节点，并判断是否为null
            if ((e = oldTab[j]) != null) {
                // 清空旧table中数据
                oldTab[j] = null;
                // e 节点next节点为null，则将e节点重新hash后赋值为新table中的节点数据
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                // 如果节点e为树节点则执行树节点的拆分方法
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                else { // preserve order
                    //关于下面这段代码执行逻辑请参考下面链接地址（1）
                    // 大概意思就是将原链表按(e.hash & oldCap) == 0 条件拆分为了两个链表lo和hi，然后将lo链表对应在新表的下标位置与旧表一致的 j 下标下，将hi链表对应在新表的下标 j + oldCap 的下标下 
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```
（1）[https://segmentfault.com/a/1190000015812438?utm_source=tag-newest](https://segmentfault.com/a/1190000015812438?utm_source=tag-newest)
