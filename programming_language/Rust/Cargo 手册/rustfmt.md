```
这个工具使用 rustfmt 格式化当前 crate 中的所有 bin 和 lib 文件。

使用方式：cargo fmt [OPTIONS] [-- <rustfmt_options>...]

参数： 
  [rustfmt_options]... 传递给 rustfmt 的选项

选项：
  -q, --quiet
          不向 stdout 输出任何内容
  -v, --verbose
          使用详细输出
      --version
          打印 rustfmt 版本并退出
  -p, --package <package>...
          指定要格式化的包
      --manifest-path <manifest-path>
          指定 Cargo.toml 的路径
      --message-format <message-format>
          指定消息格式：short|json|human
      --all
          格式化所有包，以及它们的本地基于路径的依赖项
      --check
          以检查模式运行 rustfmt
  -h, --help
          打印帮助
```