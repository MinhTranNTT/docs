# error

## error: package(s) `leet_code` not found in workspace

```
~> cargo run --package leet_code --example list
error: package(s) `leet_code` not found in workspace `D:\leet_code`
```

1. 设置 `workspace.exclude`

```toml
[workspace]
exclude = ["leet_code"]
```

2. 设置 `.vscode\settings.json`

```json
{
    "rust-analyzer.linkedProjects": [
        ".\\leet_code\\Cargo.toml",
    ]
}
```



## error: linker `cc` not found

```sh
error: linker `cc` not found
  |
  = note: No such file or directory (os error 2)

error: could not compile `ceshi` due to previous error
```

在终端执行以下命令：

```sh
sudo apt update
sudo apt install build-essential
```

## error：failed to run custom build command for `pear_codegen v0.1.4`

https://blog.csdn.net/weixin_41760738/article/details/108060293