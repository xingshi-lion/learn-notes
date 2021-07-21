一、简述

Scala语言中的集合可以分为可变与不可变集合。但是主要来自三个包：

- scala.collection
- scala.collection.immutable
- scala.collection.mutable

可变集合可以在适当的时候被更新或被扩展，这意味着你可以修改，添加，移除一个集合；

而不变的集合类，永远不会改变。不过可以模拟改变，新增、移除或者更新操作，但是实际上（书生：跟java的string一样）他返回了一个新的对象，这里面就是指返回了一个新的集合，而老的集合没有改变。

 

二、各自的优势

- immutable
- - 多线程情况下，可以保持线程安全；
  - 减少出错的可能性；
  - 减少测试的代码量；
  - 能获得更好的性能；
  - 不需要支持可变性，可以尽量的节省空间和时间的开销，所有的不可变集合实现都比可变集合更加合理有效的利用内存。
  - 可以被使用为一个常量，并且期望在未来也是保持不变的；
- mutable
- - 可以很方便的在原集合上进行增删改查

 

三、集合类

- scala.collection所有了集合类-高级抽象类（包含可变与不可变）

![img](https://www.pianshen.com/images/846/696f817ca8cc0b0bee5db971011e0c56.png)

 

- scala.collection.immutable中的所有集合类（不可变）

![img](https://www.pianshen.com/images/861/acdfa5c1830167e38a0e801bbc3d9bcd.png)

 

- scala.collection.mutable

![img](https://www.pianshen.com/images/509/f32d3fcad798276921480d7a2116b1ad.png)