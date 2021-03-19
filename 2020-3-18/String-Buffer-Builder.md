## String，StringBuffer，StringBuilder 之间有什么区别

- String 字符串常量(final修饰，不可被继承)，不可变、线程安全
- StringBuffer  可变、线程安全，其也是final类别的，不允许被继承
- StringBuilder 可变、非线程安全

#### 区别：

- StringBuffer 对方法加了同步锁、线程安全
- StringBuilder 没有对方法加同步锁，线程不安全，但StringBuilder 的性能要大于 StringBuffer。