# 安装自定义版本

如果你对在源文件中所做的更改感到满意并希望在系统中使用它们，你可以将其安装为自定义版本。

对于 macOS 和 Linux，你可以使用 `altinstall` 命令，该命令不会为 `python3` 创建符号链接，而是安装一个独立版本：

```bash
$ make altinstall
```

而对于 Windows，你必须将构建配置从 `Debug` 更改为 `Release`，然后将打包的二进制文件复制到目标目录，并确保该目录在计算机的系统变量中。