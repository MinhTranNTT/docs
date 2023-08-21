# try、catch、finally 考察，请指出下面程序的运行结果。

```java
public class TryDemo {
    public static void main(String[] args) {
        System.out.println(test());		// 31
    }
    public static int test() {
        try {
            return 1;
        } catch (Exception e) {
            return 2;
        } finally {
            System.out.print("3");
        }
    }
}
```

try、catch。finally 的基础用法，在 return 前会先执行 finally 语句块，所以是先输出 finally 里的 3，再输出 return 的 1。

```java
public class TryDemo {
    public static void main(String[] args) {
        System.out.println(test1());		// 3
    }
    public static int test1() {
        try {
            return 2;
        } finally {
            return 3;
        }
    }
}
```

try 返回前先执行 finally，结果 finally 里不按套路出牌，直接 return 了，自然也就走不到 try 里面的 return 了。

finally 里面使用 return 仅存在于面试题中，实际开发中千万不要这么用。

```java
public class TryDemo {
    public static void main(String[] args) {
        System.out.println(test1());		// 2
    }
    public static int test1() {
        int i = 0;
        try {
            i = 2;
            return i;
        } finally {
            i = 3;
        }
    }
}
```

在执行 finally 之前，JVM 会先将 i 的结果暂存起来，然后 finally 执行完毕后，会返回之前暂存的结果，而不是返回 i，所以即使这边 i 已经被修改为 3，最终返回的还是之前暂存起来的结果 2。

这边其实根据字节码可以很容易看出来，在进入 finally 之前，JVM 会使用 iload、istore 两个指令，将结果暂存，在最终返回时在通过 iload、ireturn 指令返回暂存的结果。

‍
