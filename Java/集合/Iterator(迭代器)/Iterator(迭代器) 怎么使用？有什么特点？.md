# Iterator(迭代器) 怎么使用？有什么特点？

```java
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        Iterator<String> it = list.iterator();
        while (it.hasNext()) {
            String obj = it.next();
            System.out.println(obj);
        }
    }
```

Iterator 的特点是更加安全，因为它可以确保，在当前遍历的集合元素被更改的时候，就会抛出 ConcurrentModificationException 异常。

‍
