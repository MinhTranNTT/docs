# Integer a = 128, b = 128,

[(37条消息) Integer a = 128, Integer b = 128, a](https://blog.csdn.net/shijiujiu33/article/details/83717281?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167802180016800227488969%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167802180016800227488969&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-83717281-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=Integer%20a%20%3D%20128%2C%20b%20%3D%20128%2C&spm=1018.2226.3001.4187)​==[b ; Integer c = 100 , integer d =100 , c](https://blog.csdn.net/shijiujiu33/article/details/83717281?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167802180016800227488969%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167802180016800227488969&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-83717281-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=Integer%20a%20%3D%20128%2C%20b%20%3D%20128%2C&spm=1018.2226.3001.4187)==​[d_xiaoshijiu333的博客-CSDN博客](https://blog.csdn.net/shijiujiu33/article/details/83717281?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522167802180016800227488969%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=167802180016800227488969&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-83717281-null-null.142^v73^control_1,201^v4^add_ask,239^v2^insert_chatgpt&utm_term=Integer%20a%20%3D%20128%2C%20b%20%3D%20128%2C&spm=1018.2226.3001.4187)

---

```java

public static void main(String[] args) {
    Integer a = 128, b = 128, c = 127, d = 127;
    System.out.println(a == b);		// false
    System.out.println(c == d);		// true
}
```

默认情况下范围为：-128~127

c 和 d 是相同对象，而 128 则没有命中，所以 a 和 b 是不同对象。

‍
