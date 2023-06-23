# realpath 

```bash
$ realpath --help
用法：realpath [选项]... 文件...
打印经过解析的绝对路径文件名；
除最后一部分外，文件名的所有组成部分必须存在

  -e, --canonicalize-existing  路径中的所有组成部分必须存在
  -m, --canonicalize-missing   路径中的各个组成部分若不存在则视为目录
  -L, --logical                在解析符号链接之前先处理路径中的 '..' 组成部分
  -P, --physical               遇到符号链接则立即解析（默认）
  -q, --quiet                  不输出大多数错误消息
      --relative-to=目录       相对于 <目录> 输出解析后的路径
      --relative-base=目录     输出绝对路径，除非它是 <目录> 的子目录
  -s, --strip, --no-symlinks   不要解析符号链接
  -z, --zero                   以 NUL 字符而非换行符结束每个输出行
      --help        显示此帮助信息并退出
      --version     显示版本信息并退出

GNU coreutils 在线帮助：<https://www.gnu.org/software/coreutils/>
请向 <http://translationproject.org/team/zh_CN.html> 报告任何翻译错误
完整文档 <https://www.gnu.org/software/coreutils/realpath>
或者在本地使用：info '(coreutils) realpath invocation'

```

‍
