# shell 打印第十行参数

```shell
[root@192 liuzonglin]# touch file.txt
[root@192 liuzonglin]# ls
file.txt
[root@192 liuzonglin]#
[root@192 liuzonglin]# cat > file.txt << lzl
> Line 1
> Line 2
> Line 3
> Line 4
> Line 5
> Line 6
> Line 7
> Line 8
> Line 9
> Line 10
> lzl
[root@192 liuzonglin]#
[root@192 liuzonglin]# cat file.txt
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
[root@192 liuzonglin]#
[root@192 liuzonglin]# grep 'Line 10' file.txt
Line 10
[root@192 liuzonglin]#
[root@192 liuzonglin]# awk 'NR == 10' file.txt		# NR 在 awk 中指行号
Line 10
[root@192 liuzonglin]#
[root@192 liuzonglin]# sed -n '10p' file.txt		# -n 表示只输出匹配行，p 表示 Print
Line 10
[root@192 liuzonglin]# tail -n +10 file.txt | head -1		# tail -n +10 表示从第10行开始输出
Line 10
[root@192 liuzonglin]#
```

‍
