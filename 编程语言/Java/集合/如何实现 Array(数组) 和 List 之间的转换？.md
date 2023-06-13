# 如何实现 Array(数组) 和 List 之间的转换？

* Array(数组)转 List：使用 Arrays.asList(array) 进行转换。
* List 转Array(数组)：使用 List 自带的 toArray() 方法。

‍

```java
    public static void main(String[] args) {
        // list to array
        List<String> list = new ArrayList<String>();
        list.add("王磊");
        list.add("的博客");
        list.toArray();

        // array to list
        String[] array = new String[]{"王磊", "的博客"};
        Arrays.asList(array);
    }
```

‍
