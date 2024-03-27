# 环境替代 - Environment Replacement

环境变量(使用 `ENV` 语句声明)也可以在某些指令中作为变量使用，由 `Dockerfile` 进行解析。

环境变量在 `Dockerfile` 中用 `$variable_name` 或 `${variable_name}` 表示。大括号语法通常用于解决没有空格的变量名问题，例如：`${foo}_bar`。

`${variable_name}` 语法还支持以下指定的一些标准 `bash` 修饰符：

- `${variable:-word}` 表示如果设置了变量，那么结果将是该值。如果未设置，则结果为 `word`
- `${variable:+word}` 表示如果设置了变量，则结果为 `word`，否则结果为空字符串

在任何情况下，`word` 可以是任何字符串，包括附加的环境变量。

可以通过在变量前添加 `` \ `` 来进行转义：`\$foo` 或 `\${foo}` 将分别转换为 `$foo` 和 `${foo}` 字面量。

示例：

```dockerfile
FROM busybox
ENV FOO=/bar
WORKDIR ${FOO}    # WORKDIR /bar
ADD . $FOO        # ADD . /bar
COPY \$FOO /quux  # COPY $FOO /quux
```

环境变量在以下 `Dockerfile` 指令中被支持：

- `ADD`
- `COPY`
- `ENV`
- `EXPOSE`
- `FROM`
- `LABEL`
- `STOPSIGNAL`
- `USER`
- `VOLUME`
- `WORKDIR`
- `ONBUILD`(当与上面支持的指令之一结合使用时)

环境变量替换将在整个指令中对每个变量使用相同的值。换句话说，在这个例子中:

```dockerfile
ENV abc=hello
ENV abc=bye def=$abc
ENV ghi=$abc
```

将导致 `def` 的值为 `hello`，而不是 `bye`。然而 `ghi` 的值将是 `bye`，因为它不是将 `abc` 设置为`bye` 的同一指令的一部分。