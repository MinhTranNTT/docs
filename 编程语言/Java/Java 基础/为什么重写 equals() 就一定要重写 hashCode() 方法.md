# 为什么重写 equals() 就一定要重写 hashCode() 方法

参考文档：

[(24条消息) 为什么重写equals方法必须重写hashcode方法_贤云Ye鹤的博客-CSDN博客](https://blog.csdn.net/m6494854/article/details/128572229)

[详解hashcode方法的作用 - 电子电路图,电子技术资料网站 (elecfans.com)](https://www.elecfans.com/code/java/20170926555607.html)

---

	equals在object类里的作用是**比较两个引用指向的是否是同一个对象**, 也就是两个对象的地址是否相同，但是我们在实际开发过程中,这种开发方式往往不满足我们的需求,比如我们有一个person类,我们的需求可能是当一个人的年龄和姓名和性别都相同时,我们就认为这是同一个人所以我们需要重写equals方法来自行决定两个对象相等的判别方式 。但是在hashMap的底层判断两个对象是否相同，是需要hashcode和equals同时来决定的，先用hashcode进行比较找到大概的位置，如果相等了就使用equals进行精确判断，如果不相等就不用再次判断了，减少使用equals开销，所以如果我们只重写了equals但是没有重写hashcode,就可能会出现，equals比较相等，但是hashcode不等的情况，两个对象equals相等的话，hashcode值是必然相等的，这就产生了矛盾。  

---

‍

## HashCode的作用：

	对于包含容器类型的程序设计语言来说，基本上都会涉及到hashCode。在 Java 中也一样，hashCode方法的主要作用是为了配合基于散列的集合一起正常运行，这样的散列集合包括HashSet、HashMap以及HashTable。

　　为什么这么说呢？考虑一种情况，当向集合中插入对象时，如何判别在集合中是否已经存在该对象了？（注意：集合中不允许重复的元素存在）

　　也许大多数人都会想到调用equals方法来逐个进行比较，这个方法确实可行。但是如果集合中已经存在一万条数据或者更多的数据，如果采用equals方法去逐一比较，效率必然是一个问题。此时hashCode方法的作用就体现出来了，当集合要添加新的对象时，先调用这个对象的hashCode方法，得到对应的hashcode值，实际上在HashMap的具体实现中会用一个table保存已经存进去的对象的hashcode值，如果table中没有该hashcode值，它就可以直接存进去，不用再进行任何比较了；如果存在该hashcode值， 就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址，所以这里存在一个冲突解决的问题，这样一来实际调用equals方法的次数就大大降低了，说通俗一点：Java中的hashCode方法就是根据一定的规则将与对象相关的信息（比如对象的存储地址，对象的字段等）映射成一个数值，这个数值称作为散列值。下面这段代码是`java.util.HashMap`​的中`put()`​方法的具体实现：

‍

​`put()`​方法是用来向HashMap中添加新的元素，从`put()`​方法的具体实现可知，会先调用`hashCode()`​方法得到该元素的hashCode值，然后查看table中是否存在该hashCode值，如果存在则调用`equals()`​方法重新确定是否存在该元素，如果存在，则更新`value`​值，否则将新的元素添加到HashMap中。从这里可以看出，hashCode方法的存在是为了减少equals方法的调用次数，从而提高程序效率。

‍

‍
