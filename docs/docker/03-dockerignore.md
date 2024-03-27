# .dockerignore 文件

在 Docker CLI 将上下文发送给 Docker 守护进程之前，它会在上下文的根目录中查找一个名为 `.dockerignore` 的文件。如果该文件存在，CLI 将修改上下文以排除与该文件中的模式匹配的文件和目录。这有助于避免将不必要的大型或敏感文件和目录发送到守护进程，并且避免被 `ADD` 或 `COPY` 指令将它们添加到镜像中。

示例：

```dockerignore
# 注释
*/temp*
*/*/temp*
**/temp*
temp?
```

该文件将导致以下构建行为：

| 规则        | 行为                                                                           |
| ----------- | ------------------------------------------------------------------------------ |
| `# 注释`    | 忽略                                                                           |
| `*/temp*`   | 排除根目录的任何直接子目录中以 `temp` 开头的文件和目录                         |
| `*/*/temp*` | 排除比根目录低两级的任何子目录中以 `temp` 开头的文件和目录                     |
| `**/temp*`  | 排除任何目录中以 `temp` 开头的文件和目录                                       |
| `temp?`     | 排除根目录下以 `temp` 为一个字符扩展名的文件和目录，例如：`/tempa` 和 `/tempb` |

以 `!` 开头的行可以用来表示排除的例外：

```dockerignore
*.md
!README.md
```

除 `README.md` 以外的 markdown 文件从上下文中排除。

`!` 的位置对排除行为的影响：与特定文件匹配的 `.dockerignore` 的最后一行决定它是被包含还是被排除。

示例：

```dockerignore
*.md
!README*.md
README-secret.md
```

除了 `README-secret.md` 以外，任何 README 文件都不会包含到上下文中。

另一个示例：

```dockerignore
*.md
README-secret.md
!README*.md
```

所有的 README 文件都包括在内。中间行没有效果，因为 `!README*.md` 是最后一个，它匹配 `README-secret.md`。

你甚至可以使用 `.dockerignore` 文件来排除 `Dockerfile` 和 `.dockerignore` 文件。这些文件仍然发送给守护进程，因为它需要它们来完成它的工作，但是 `ADD` 和 `COPY` 指令不会将它们复制到镜像中。

:::note
由于历史原因，规则 `.` 被忽略。
:::
