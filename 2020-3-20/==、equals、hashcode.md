# ==、equals、hashcode

- == 比地址（堆内存地址）、equals比值

  > equals 若没有被重写，跟 == 一样也是比地址；如果 equals()⽅法被重写（例如 String），则比较的是地址⾥的内容

- hashCode() 的作⽤是获取哈希码，也称为散列码；它实际上是返回⼀个 int 整数。这个哈希码的 作⽤是确定该对象在哈希表中的索引位置。 hashCode() 定义在 JDK 的 Object 类中，这就意味 着 Java 中的任何类都包含有 hashCode() 函数。

##### 为什么要有 hashCode？

以“ HashSet 如何检查重复”为例⼦来说明为什么要有 hashCode？

当你把对象加⼊ HashSet 时， HashSet 会先计算对象的 hashcode 值来判断对象加⼊的位置， 同时也会与其他已经加⼊的对象的 hashcode 值作⽐较，如果没有相符的 hashcode， HashSet 会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调⽤ equals() ⽅ 法来检查 hashcode 相等的对象是否真的相同。如果两者相同， HashSet 就不会让其加⼊操作成 功。如果不同的话，就会重新散列到其他位置。

这样就⼤⼤减少了 equals 的次数，相应就⼤⼤提⾼了执⾏速度。

##### 为什么两个对象有相同的 hashcode 值，它们也不⼀定是相等的？

因为 hashCode() 所使⽤的杂凑算法也许刚好会让多个对象传回相同的杂凑值。越糟糕的杂凑算 法越容易碰撞，但这也与数据值域分布的特性有关（所谓碰撞也就是指的是不同的对象得到相同 的 hashCode 。



> hashCode() 与 equals() 的相关规定： 1. 如果两个对象相等，则 hashcode ⼀定也是相同的 2. 两个对象相等,对两个 equals() ⽅法返回 true 3. 两个对象有相同的 hashcode 值，它们也不⼀定是相等的 4. 综上， equals() ⽅法被覆盖过，则 hashCode() ⽅法也必须被覆盖 5. hashCode() 的默认⾏为是对堆上的对象产⽣独特值。如果没有重写 hashCode() ，则该 class 的两个对象⽆论如何都不会相等（即使这两个对象指向相同的数据）。