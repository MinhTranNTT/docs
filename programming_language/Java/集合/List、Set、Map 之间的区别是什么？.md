# List、Set、Map 之间的区别是什么？

List、Set、Map 的区别主要体现在两个方面：元素是否有序、是否允许元素重复。

‍

|<br />|<br />|元素有序|允许元素重复|
| :-----: | :-------------------------: | :-------------: | :------------------: |
|List||是|是|
|Set<br />|AbstractSet<br />|否|否<br />|
||HashSet|||
||TreeSet|是（用二叉树排序）||
|Map<br />|AbstractMap|否|Key 值必须唯一，Value 可重复|
||HashMap|||
||TreeMap|是（用二叉树排序）||

‍

‍
