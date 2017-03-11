title: jdk源码阅读——ArrayList
date: 2016-11-09 23:49:00
categories: jdk源码阅读
tags: [源码阅读,jdk]
toc: true
---
>个人观点：集合类和concurrent下的类是java里特别精髓的东西。
<!--more-->
# 概要

- 类的定义

```
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

- 类的继承关系

```
java.lang.Object
    java.util.AbstractCollection<E>
        java.util.AbstractList<E>
            java.util.ArrayList<E>
```

- 类的特点

http://blog.csdn.net/u014394255/article/details/53449122

# 属性
    
```
private static final int DEFAULT_CAPACITY = 10;
```
此属性定义list的默认空间大小


```
transient Object[] elementData;
```
此属性有序缓存数组元素，每增加一个元素，则，用transient修饰，表示不会序列化

# 方法
- 初始化

```
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
```
用指定的初始化容量初始化一个空的list


- toArray

```
public Object[] toArray() {
        return Arrays.copyOf(elementData, size);
    }
```

- indexOf

```
public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
```


检查list中元素是否为null,或在list中的index

- elementData

```
@SuppressWarnings("unchecked")
    E elementData(int index) {
        return (E) elementData[index];
    }
```

返回访问操作元素位置对应的value

- add

```
public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        size++;
    }
```
添加参数到list中，rangeCheckForAdd方法会index的上下界进行检查

- remove

```
public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
```
移除list指定位置的值，并释放gc

- 线程安全

```
private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException{
        // Write out element count, and any hidden stuff
        int expectedModCount = modCount;
        s.defaultWriteObject();

        // Write out size as capacity for behavioural compatibility with clone()
        s.writeInt(size);

        // Write out all elements in the proper order.
        for (int i=0; i<size; i++) {
            s.writeObject(elementData[i]);
        }

        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
    }
```
- 容量扩充


```
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

设置最小的容量，来扩充list的大小；可以看出，oldCapacity 新增的容量是它的一半。另外，还有一个 hugeCapacity，如果需要扩充的容量比　MAX_ARRAY_SIZE 还大，会调用这个函数，重新调整大小。但再大也大不过　Integer.MAX_VALUE。

