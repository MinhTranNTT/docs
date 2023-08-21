# Git 乱码

参考文档：

[gitbash乱码解决办法-蒲公英云 (dandelioncloud.cn)](https://dandelioncloud.cn/article/details/1488358869889044481/)

[(22条消息) git status中文文件名乱码_Linux小百科的博客-CSDN博客_git status 中文乱码](https://blog.csdn.net/yaxuan88521/article/details/127597685)

# Git 乱码

### Windows Git 乱码

![img](assets/Git%20%E4%B9%B1%E7%A0%81/922b11d24c184ff6b58ca993938ffdd7-20230113191333-hf5023e.png)

### git status 中文文件名乱

```shell
$ git status # 中文文件名乱码
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    "../Git/Windows Git \344\271\261\347\240\201.md"

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        "../Git/Git \344\271\261\347\240\201.md"

no changes added to commit (use "git add" and/or "git commit -a")

liuzonglin@LAPTOP-CGO0UV3J MINGW64 /d/.github/.doc/Maven (master)
$ git config --global core.quotepath false

liuzonglin@LAPTOP-CGO0UV3J MINGW64 /d/.github/.doc/Maven (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    ../Git/Windows Git 乱码.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        ../Git/Git 乱码.md

no changes added to commit (use "git add" and/or "git commit -a")

liuzonglin@LAPTOP-CGO0UV3J MINGW64 /d/.github/.doc/Maven (master)
```

‍
