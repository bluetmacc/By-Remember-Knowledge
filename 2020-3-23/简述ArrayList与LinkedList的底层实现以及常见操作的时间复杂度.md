## 简述 ArrayList 与 LinkedList 的底层实现以及常见操作的时间复杂度

#### ArrayList

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

- 基于数组实现，所以支持快速随机访问

- 数组的默认大小为 10

```JAVA
private static final int DEFAULT_CAPACITY = 10;
```

- 添加元素时使用 ensureCapacityInternal() 方法来保证容量足够，如果不够时，需要使用 grow() 方法进行扩容，新容量的大小为 `oldCapacity + (oldCapacity >> 1)`，即 oldCapacity+oldCapacity/2。其中 oldCapacity >> 1 需要取整，所以新容量大约是旧容量的 1.5 倍左右。（oldCapacity 为偶数就是 1.5 倍，为奇数就是 1.5 倍-0.5）
- 扩容操作需要调用 `Arrays.copyOf()` 把原数组整个复制到新数组中，这个操作代价很高，因此最好在创建 ArrayList 对象时就指定容量大小，减少扩容操作的次数。

- 删除元素：调用 System.arraycopy() 将 index+1 后面的元素都复制到 index 位置上，该操作的时间复杂度为 O(N)，可以看到 ArrayList 删除元素的代价是非常高的。

```java
public E remove(int index) {
    rangeCheck(index);
    modCount++;
    E oldValue = elementData(index);
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index, numMoved);
    elementData[--size] = null; // clear to let GC do its work
    return oldValue;
}
```

#### LinkedList

- 基于双向链表实现，使用 Node 存储链表节点信息。（jdk1.6之前循环、jdk1.7取消）

> 双向链表：包含两个指针，⼀个 prev 指向前⼀个节点，⼀个 next 指向后⼀个节点
>
> 双向循环链表： 最后⼀个节点的 next 指向 head，⽽ head 的 prev 指向最后⼀个节点，构成⼀ 个环。

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}
```

- 插入删除是否受元素位置影响：
  - Array用数组存储、若增加元素到列末尾、时间复杂度O1，若要在指定位置i插入删除、时间复杂度On-i
  - Linked用链表存储、若不指定位置插入删除 时间复杂度O1，若指定位置i插入删除、时间复杂度On


####  与 ArrayList 的比较

ArrayList 基于动态数组实现，LinkedList 基于双向链表实现。ArrayList 和 LinkedList 的区别可以归结为数组和链表的区别：

- 数组支持快速随机访问，但插入删除的代价很高，需要移动大量元素；
- 链表不支持快速随机访问，但插入删除只需要改变指针。

> 快速随机访问：通过元素的序号快速获取元素对象（对应get(int ndex)）

