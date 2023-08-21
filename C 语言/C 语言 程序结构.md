## C Hello 实例

```c
#include <stdio.h>

int main()
{
    /* 我的第一个 C 程序 */
    printf("Hello, W3Cschool! \n");

    return 0;
}
```

编译代码

```sh
gcc hello.c
```

运行代码 ./main.exe

```sh
gcc hello.c -o main.exe
```

代码解读

```c
printf                     标识符
(                          符号
"Hello, W3Cschool! \n"     字符串值
)                          符号
;                          分号是语句结束符
```