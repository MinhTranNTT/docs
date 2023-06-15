# Iterator(迭代器) 和 ListIterator 有什么区别？

Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List。  
Iterator 只能单向遍历，而 ListIterator 可以双向遍历（向前/后遍历）。  
ListIterator 从 Iterator 接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

‍
