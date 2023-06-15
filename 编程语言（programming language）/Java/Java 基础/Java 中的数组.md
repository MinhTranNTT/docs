# Java 中的数组

数组是一个连续的存储结构。

数组是一个对象，不同类型的数组具有不同的类。

Java中不存在int *a这样的东西做数组的形参。

可以二维数组，且可以有多维数组，都是`java`​中合法的。

```java
    public static void main(String[] args) {
        int[][] ints = {{1, 3, 3}, {5, 3, 2}, {7, 3, 5}, {7, 7, 0}};
        for (int[] a : ints) {
            for (int b : a) {
                System.out.print(b);
            }
        }
    }
```

‍
