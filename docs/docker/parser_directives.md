# 解析器指令 Parser Directives

- 解析器指令影响对 `Dockerfile` 中后续行的处理方式
- 解析器指令不会增加构建层，也不会展示在构建步骤
- 解析器指令是一种以特殊的注释编写的：`# directive=value`
- 一个指令只可以用一次

`Dockerfile` 支持以下解析器指令：

- `syntax`
- `escape`

## syntax

示例：

```dockerfile
# syntax=docker/dockerfile:1.0
```

此功能仅在使用 `BuildKit` 后端时可用。

`syntax` 指令定义用于构建当前 `Dockerfile` 的构建器的位置。BuildKit 后端允许无缝使用构建器的外部实现，这些构建器以 Docker 镜像的形式分发并在容器沙箱环境中执行。

自定义 `Dockerfile` 实现允许你：

- 自动获取错误修复，无需更新 Docker 守护程序
- 确保所有用户都使用相同的实现来构建你的 `Dockerfile`
- 在不更新 Docker 守护程序的情况下使用最新功能
- 在新功能或第三方功能集成到 Docker 守护程序之前试用他们

## escape

```dockerfile
# escape=\
```

或

```dockerfile
# escape=`
```

该指令设置 `Dockerfile` 中用于转义字符的字符。如果未指定，默认转义字符为 `\`。

转义字符用于转义一行中的字符，也用于转义换行符，这允许 `Dockerfile` 指令跨域多行。请注意，无论 `Dockerfile` 中是否包含转义解析器指令，在 `RUN` 命令中都不会执行转义，除非在一行的末尾。

将转义字符设置为 `` ` `` 在 `Windows` 容器中特别有用，因为 `` \ `` 是目录路径分隔符。

```dockerfile
FROM microsoft/nanoserver
COPY testfile.txt c:\\
RUN dir c:\
```

以上 `Dockerfile` 在构建时会报错：

```
GetFileAttributesEx c:RUN: The system cannot find the file specified.
```

加入 `escape` 指令：

```dockerfile
# escape=`

FROM microsoft/nanoserver
COPY testfile.txt c:\
RUN dir c:\
```

现在可以正常构建。
