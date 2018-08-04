---
title: HashMap源码解析
categories: "Java基础"
tags: "集合源码"
---



# HashMap源码

## 1.构造器

```java
/**
构造器默认两个参数：initialCapacity 哈希表（键值对数组）初始化容量（默认为16,2的4次方
			     loadFactor 加载因子 （默认为0.75）
*/
public HashMap(int initialCapacity, float loadFactor) {
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal initial capacity: " +
                                               initialCapacity);
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        if (loadFactor <= 0 || Float.isNaN(loadFactor))
            throw new IllegalArgumentException("Illegal load factor: " +
                                               loadFactor);

        this.loadFactor = loadFactor;
        threshold = initialCapacity;
        init();
    }
```

<!--more-->

## 2.方法

```java
    void addEntry(int hash, K key, V value, int bucketIndex) {
      //判断是否需要对原先的hash表扩容
        if ((size >= threshold) && (null != table[bucketIndex])) {
          //对hash表扩容 并将旧hash表的内容放入新hash表中
            resize(2 * table.length);
          //通过hash()方法来获取key的hash值
            hash = (null != key) ? hash(key) : 0;
          //根据hash值重新获取Entry在数组中的index
            bucketIndex = indexFor(hash, table.length);
        }
		//新增一个键值对 在hash表中的bucketIndex位置放入一个Entry
        createEntry(hash, key, value, bucketIndex);
    }
```

```java
    void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }
        Entry[] newTable = new Entry[newCapacity];
      //将所有的旧hash表的键值对转换到新hash表上
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        table = newTable;
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
    }
```

```java
   public void clear() {
        modCount++;
     //使用Arrays工具类，将hash表的数据都填充成null
        Arrays.fill(table, null);
        size = 0;
    }
```

```java
    final Entry<K,V> getEntry(Object key) {
        if (size == 0) {
            return null;
        }
	  //获取key的hash值
        int hash = (key == null) ? 0 : hash(key);
      //遍历在hash表中key的hash值所对应位置的链表 通过key.equasls方法来确定key所对应的键值对
        for (Entry<K,V> e = table[indexFor(hash, table.length)];
             e != null;
             e = e.next) {
            Object k;
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                return e;
        }
        return null;
    }
```

```java
    public V put(K key, V value) {
        if (table == EMPTY_TABLE) {
        //给hash表扩容，若表为空，则按照初始化时的threshold值创建hash表。
        //threshold=capacity * loadFactor
        //若不为空，则该方法扩容为原来hash表的2倍
            inflateTable(threshold);
        }
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key);
        int i = indexFor(hash, table.length);
      //若根据key的hash值计算出的hash表位置已经存在了键值对，则遍历该链表，将新的Entry添加到链表的最后
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
	 //将hash表的修改次数+1 为了实现fast-fail机制
        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }
```

```java
    final Entry<K,V> removeEntryForKey(Object key) {
        if (size == 0) {
            return null;
        }
        int hash = (key == null) ? 0 : hash(key);
        int i = indexFor(hash, table.length);
      //指向链表当前对象前一个对象的引用
        Entry<K,V> prev = table[i];
      //指向链表当前对象的引用
        Entry<K,V> e = prev;

        while (e != null) {
          //遍历链表数据直至找到key对应的Entry
          //next为指向链表当前数据下一个对象的引用
            Entry<K,V> next = e.next;
            Object k;
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k)))) {
              //hash表修改次数+1
                modCount++;
              //hash表的大小-1
                size--;
              //此时当前一个链表前一个数据与当前数据为同一个对象，说明hash表当前位置不存在链表，
              //直接将key所对应hash表当前数据的next引用指向当前数据的下一个对象。（链表的删除思想）
                if (prev == e)
                    table[i] = next;
                else
                  //此时说明hash表当前位置存在链表，将前一个数据的next引用指向当前数据的下一个对象
                    prev.next = next;
              //这是Entry内部类定义的hook方法，每次删除数据都需要调用一次。
                e.recordRemoval(this);
                return e;
            }
            prev = e;
            e = next;
        }
        return e;
    }
```

hook方法就是钩子方法

## 3. Entry类

```java
//静态内部类 实现了Map类的内部接口 用于存储键值对
static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        Entry<K,V> next;
        int hash;

        /**
         * Creates new entry.
         */
        Entry(int h, K k, V v, Entry<K,V> n) {
            value = v;
            next = n;
            key = k;
            hash = h;
        }

        public final K getKey() {
            return key;
        }

        public final V getValue() {
            return value;
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }
	//复写equals方法
        public final boolean equals(Object o) {
            if (!(o instanceof Map.Entry))
                return false;
            Map.Entry e = (Map.Entry)o;
            Object k1 = getKey();
            Object k2 = e.getKey();
            if (k1 == k2 || (k1 != null && k1.equals(k2))) {
                Object v1 = getValue();
                Object v2 = e.getValue();
                if (v1 == v2 || (v1 != null && v1.equals(v2)))
                    return true;
            }
            return false;
        }

        public final int hashCode() {
            return Objects.hashCode(getKey()) ^ Objects.hashCode(getValue());
        }

        public final String toString() {
            return getKey() + "=" + getValue();
        }

        /**
         * This method is invoked whenever the value in an entry is
         * overwritten by an invocation of put(k,v) for a key k that's already
         * in the HashMap.
         */
        void recordAccess(HashMap<K,V> m) {
        }

        /**
         * This method is invoked whenever the entry is
         * removed from the table.
         */
        void recordRemoval(HashMap<K,V> m) {
        }
    }

```



## 4.迭代器

```java
    //通过一个抽象内部类HashIterator实现了Iterator接口 
    //HashMap提供的key和value迭代器都是通过继承这个HashIterator实现的
	private abstract class HashIterator<E> implements Iterator<E> {
        Entry<K,V> next;        // next entry to return
        int expectedModCount;   // For fast-fail
        int index;              // current slot
        Entry<K,V> current;     // current entry

        HashIterator() {
            expectedModCount = modCount;
            if (size > 0) { // advance to first entry
                Entry[] t = table;
                while (index < t.length && (next = t[index++]) == null)
                    ;
            }
        }

        public final boolean hasNext() {
            return next != null;
        }

        final Entry<K,V> nextEntry() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            Entry<K,V> e = next;
            if (e == null)
                throw new NoSuchElementException();

            if ((next = e.next) == null) {
                Entry[] t = table;
              //当next为null时说明e是hash表当前index位置的链表的最后一个元素
              //通过while语句内的方式实现next往后移位直至不为null的一个元素
                while (index < t.length && (next = t[index++]) == null)
                    ;
            }
            current = e;
            return e;
        }

        public void remove() {
            if (current == null)
                throw new IllegalStateException();
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            Object k = current.key;
            current = null;
            HashMap.this.removeEntryForKey(k);
            expectedModCount = modCount;
        }
    }
```

```java
	//value迭代器内部类
	private final class ValueIterator extends HashIterator<V> {
        public V next() {
            return nextEntry().value;
        }
    }
	//key迭代器内部类
    private final class KeyIterator extends HashIterator<K> {
        public K next() {
            return nextEntry().getKey();
        }
    }
	//Entry迭代器内部类
    private final class EntryIterator extends HashIterator<Map.Entry<K,V>> {
        public Map.Entry<K,V> next() {
            return nextEntry();
        }
    }
```

## 5.关于hash表的两个问题

- 如何设计才能使减小hash冲突？

针对如何获取在hash表中的位置，HashMap中主要通过三个部分来减小散列冲突：

> 1. 第一部分，首先根据Object.hashcode得到一个散列值，Object.hashCode是一个native方法。一般情况下可以认为是该对象的地址信息散列得到的，也就是相当于是对象的ID，同一个对象有相同的ID。这样得到的散列值还是比较合理的.

```java
    final int hash(Object k) {
        int h = hashSeed;
        //对于String的hashCode需要另外计算
        if (0 != h && k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }
	  
        h ^= k.hashCode();

        // This function ensures that hashCodes that differ only by
        // constant multiples at each bit position have a bounded
        // number of collisions (approximately 8 at default load factor).
        //二次散列
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }
```

> 第二部分，桶的数量设计。一般由哈希值得到桶的位置都是将哈希值除以桶的数量得到的余数就是桶的位置。一般来说想要尽可能的减少散列冲突有两类办法，一类是使用素数数量的桶，例如hashTable，一类是使用2的幂次数量的桶，例如hashmap，hashmap使用2的次幂的桶有个好处，就是可以用位运算来算，只要将散列值和桶的数量-1相与就是桶的位置不需要除。这样相对来说速度快一些。hashmap里有个静态方法indexof就是用来做这个的。具体下文会说到。

```java
    static int indexFor(int h, int length) {
        // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
        //长度为2的幂次的hash表，用位运算来将散列值和桶的数量-1相与 就是数组的index,这样比采用除更快
      	//比如length是4，那如果h是0-3则返回的值就是0-3，如果是h=4则返回0，h=5则返回1
      	//length-1是因为数组下标从0开始
        return h & (length-1);
    }

```

> 第三部分，hashseed，第二部分中曾经说道通过将哈希值与桶的数量-1相与得到桶的位置。但是这样做有一个小的问题。当哈希值非常大，而桶的数量很小的时候回出现仅仅依靠哈希值的低位来散列的结果。这样即使散列值做的很好耶没有办法得到很好的散列。这时hashseed的作用就体现出来的，hashseed通过右移部分哈希值，然后将其亦或得到的结果进行在进行定位桶的位置。这样做就综合考虑了高位和低位的值。从而减小了散列冲突的可能性。此外由于java的语言特性，对于String的情况其hashseed需要额外设计。

```java
private static class Holder {

        /**
         * Table capacity above which to switch to use alternative hashing.
         */
        static final int ALTERNATIVE_HASHING_THRESHOLD;

        static {
            String altThreshold = java.security.AccessController.doPrivileged(
                new sun.security.action.GetPropertyAction(
                    "jdk.map.althashing.threshold"));

            int threshold;
            try {
                threshold = (null != altThreshold)
                        ? Integer.parseInt(altThreshold)
                        : ALTERNATIVE_HASHING_THRESHOLD_DEFAULT;

                // disable alternative hashing if -1
                if (threshold == -1) {
                    threshold = Integer.MAX_VALUE;
                }

                if (threshold < 0) {
                    throw new IllegalArgumentException("value must be positive integer.");
                }
            } catch(IllegalArgumentException failed) {
                throw new Error("Illegal value for 'jdk.map.althashing.threshold'", failed);
            }
		
            ALTERNATIVE_HASHING_THRESHOLD = threshold;
        }
    }
```

`static final int ALTERNATIVE_HASHING_THRESHOLD_DEFAULT = Integer.MAX_VALUE;`但是这个静态类常量也是可以根据虚拟机参数的设定来更改的。这也就是Holder这个类这段静态代码的意义了。可以通过调整虚拟机的参数来设定这个域值。 
关于Holder类是参考别人的文章来的，说实话我也没有完全搞清楚。

- 在发生hash冲突时，如何解决？

在发生hash冲突之后，HashMap采用单向链表方式来存储键值对。在前面介绍的`getEntry`、`put`等方法的时候，都有遍历链表。

## 关于final

加上final的仅仅是相当于当前的引用不在改变，但是容器的元素是恶意增删的，元素的内容也是可以改变的。

## 参考链接

[参考文章一](https://blog.csdn.net/u011518120/article/details/53640181)

