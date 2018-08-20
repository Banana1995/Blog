---
title: ArrayList源码解析
date: 2018-05-13 19:40:59
categories: "Java基础"
tags: "集合源码"
---



# ArrayList源码



## field

```java
private static final int DEFAULT_CAPACITY = 10;//默认的数组容量
private static final Object[] EMPTY_ELEMENTDATA = {};//初始化时加载一个空数组
private transient Object[] elementData;//实际用来存储数据的数组
private int size;//数组大小
```

## 构造器

```java
   public ArrayList() {
        super();
        this.elementData = EMPTY_ELEMENTDATA;
    }
```

<!--more-->

默认的无参构造器就是简单的将数组引用指向类定义的空数组对象。

```java
    public ArrayList(Collection<? extends E> c) {
      //将集合转换为存储数据的数组
        elementData = c.toArray();
        size = elementData.length;
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
      //若不是Object类型的数组，则将数组copy到Object类型的数组中去
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    }
```

## 方法

1. 新增方法

   ```java
       public void add(int index, E element) {
       	//确保index的位置没有超出数组的范围
           rangeCheckForAdd(index);
   		//保证数组的容量是否足以添加进去元素，并且将modCount的值自增1
           ensureCapacityInternal(size + 1);  // Increments modCount!!
         	//将index位置后面的数据往后移1位
           System.arraycopy(elementData, index, elementData, index + 1,
                            size - index);
         	//将元素放置在index位置上
           elementData[index] = element;
         	//数据长度+1
           size++;
       }
   ```

   > fail-fast机制在遍历一个集合时，当集合结构被修改，会抛出Concurrent Modification Exception。
   >
   > fail-fast会在以下两种情况下抛出ConcurrentModificationException
   >
   > （1）单线程环境
   >
   > 集合被创建后，在遍历它的过程中修改了结构。
   >
   > 但是迭代器的remove()方法会让expectModcount和modcount 相等，所以在遍历集合的过程中只能通过迭代器的remove()方法进行删除元素。
   >
   > （2）多线程环境
   >
   > 当一个线程在遍历这个集合，而另一个线程对这个集合的结构进行了修改。

   ```java
     public boolean add(E e) {
           ensureCapacityInternal(size + 1);  // Increments modCount!!
           elementData[size++] = e;
           return true;
       }
   ```

   ```java
   private void ensureCapacityInternal(int minCapacity) {
           if (elementData == EMPTY_ELEMENTDATA) {
               minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
           }

           ensureExplicitCapacity(minCapacity);
       }

       private void ensureExplicitCapacity(int minCapacity) {
         //将修改次数+1
           modCount++;
           // overflow-conscious code
         	//增加数组长度
           if (minCapacity - elementData.length > 0)
               grow(minCapacity);
       }

       private void grow(int minCapacity) {
           // overflow-conscious code
           int oldCapacity = elementData.length;
         	//新数组长度为原先数组长度的1.5倍
           int newCapacity = oldCapacity + (oldCapacity >> 1);
         	//若新长度依旧小于minCapacity，则将minCapacity作为新长度
           if (newCapacity - minCapacity < 0)
               newCapacity = minCapacity;
         	//新长度大于MAX_ARRAY_SIZE，则将新长度置为最大的int值
           if (newCapacity - MAX_ARRAY_SIZE > 0)
               newCapacity = hugeCapacity(minCapacity);
         	//将就数组copy到新数组上
           // minCapacity is usually close to size, so this is a win:
           elementData = Arrays.copyOf(elementData, newCapacity);
       }

       private static int hugeCapacity(int minCapacity) {
           if (minCapacity < 0) // overflow
               throw new OutOfMemoryError();
         //MAX_ARRAY_SIZE的值为Integer.MAX_VALUE - 8
           return (minCapacity > MAX_ARRAY_SIZE) ?
               Integer.MAX_VALUE :
               MAX_ARRAY_SIZE;
       }
   ```

2. get方法

   ```java
       public E get(int index) {
       	//确保index的位置没有超出数组的范围
           rangeCheck(index);
           return elementData(index);
       }
   ```

3. remove方法

   ```java
       public E remove(int index) {
         	//检查是否数组下标越界
           rangeCheck(index);
   		//修改次数+1
           modCount++;
           E oldValue = elementData(index);

           int numMoved = size - index - 1;
           if (numMoved > 0)
             	//将index位置后面的数组往前移1位
               System.arraycopy(elementData, index+1, elementData, index,
                                numMoved);
         	//将size的值-1，并将数组最后一位的数据引用置为null 让GC自动回收未被引用的对象
           elementData[--size] = null; // clear to let GC do its work
   		//将被移除的对象返回出去
           return oldValue;
       }
   ```

   ```java
       public boolean remove(Object o) {
           if (o == null) {
             //若是空对象的话则遍历数组将其移除
               for (int index = 0; index < size; index++)
                   if (elementData[index] == null) {
                       fastRemove(index);
                       return true;
                   }
           } else {
             //不是空对象则遍历数组，通过equals方法判断对象是否相等，相等则将其移除
               for (int index = 0; index < size; index++)
                   if (o.equals(elementData[index])) {
                       fastRemove(index);
                       return true;
                   }
           }
           return false;
       }
   ```

   ```java
       private void fastRemove(int index) {
         	//修改次数+1
           modCount++;
           int numMoved = size - index - 1;
           if (numMoved > 0)
             //将index位置后面的数组往前移1位
               System.arraycopy(elementData, index+1, elementData, index,
                                numMoved);
         	//将size的值-1，并将数组最后一位的数据引用置为null 让GC自动回收未被引用的对象
           elementData[--size] = null; // clear to let GC do its work
       }

   ```

   ```java
       public void clear() {
       	//修改次数+1
           modCount++;
   	    //将所有的数组引用都置为null 让GC回收
           // clear to let GC do its work
           for (int i = 0; i < size; i++)
               elementData[i] = null;
   	    //数组大小置为0
           size = 0;
       }
   ```

   ​由上面源码可以看出，ArrayList的删除操作本质上都是将数组移位，末尾数组引用置为null，让GC自动回收垃圾对象。

## 迭代器

```java
 private class Itr implements Iterator<E> {
        int cursor;       // index of next element to return
        int lastRet = -1; // index of last element returned; -1 if no such
        int expectedModCount = modCount;

        public boolean hasNext() {
            return cursor != size;
        }

        @SuppressWarnings("unchecked")
        public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }

        public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }
```

先是一个`Itr`的类，实现了迭代器接口。

```java
   private class ListItr extends Itr implements ListIterator<E> {
        ListItr(int index) {
            super();
            cursor = index;
        }

        public boolean hasPrevious() {
            return cursor != 0;
        }

        public int nextIndex() {
            return cursor;
        }

        public int previousIndex() {
            return cursor - 1;
        }

        @SuppressWarnings("unchecked")
        public E previous() {
            checkForComodification();
            int i = cursor - 1;
            if (i < 0)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i;
            return (E) elementData[lastRet = i];
        }

        public void set(E e) {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.set(lastRet, e);
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }

        public void add(E e) {
            checkForComodification();

            try {
                int i = cursor;
                ArrayList.this.add(i, e);
                cursor = i + 1;
                lastRet = -1;
                expectedModCount = modCount;
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }
    }
```

然后通过一个`ListItr`继承`Itr`类，并同时实现了`ListIterator`接口。 

## 参考文章

[参考文章一](https://blog.csdn.net/ch717828/article/details/46892051)

