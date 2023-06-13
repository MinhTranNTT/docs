# 在 Queue 中 poll() 和 remove()有什么区别？

* 相同点：都是返回第一个元素，并在队列中删除返回的对象。
* 不同点：如果没有元素 poll() 会返回 null，而 remove() 会直接抛出 NoSuchElementException 异常。

‍

```java
    public static void main(String[] args) {
        Queue<String> queue = new LinkedList<String>();
        queue.offer("string"); // add
        System.out.println(queue.poll());
        System.out.println(queue.remove());
        System.out.println(queue.size());
    }
```

‍

执行结果

```cmd
string
Exception in thread "main" java.util.NoSuchElementException
	at java.base/java.util.LinkedList.removeFirst(LinkedList.java:274)
	at java.base/java.util.LinkedList.remove(LinkedList.java:689)
	at org.example.Main.main(Main.java:10)
```

‍
