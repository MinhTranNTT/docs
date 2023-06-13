# Rust加快cargo run的下载速度

**加快源速度**

找到当前用户目录下 .cargo文件夹，建立config文件：

```bash
touch ~/.cargo/config
vim ~/.cargo/config
```

输入下面内容：

```bash
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"

replace-with = 'tuna'
[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"


[net]
git-fetch-with-cli = true
```

‍
