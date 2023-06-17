# Java 中的 Math.round(-1.5) 等于多少？

等于 -1，因为在数轴上取值时，中间值（0.5）向右取整，所以正 0.5 是往上取整，负 0.5 是直接舍弃。

‍

```java
    public static void main(String[] args) {
        System.out.printf(
                "Math.round(-1.5) 值为：%d%n",
                Math.round(-1.5)
        );
    }
```

‍

**执行结果**

```cmd
Math.round(-1.5) 值为：-1
```

‍
