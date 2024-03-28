# facl 设置文件/目录权限的继承

默认情况下，新创建的文件拥有者为创建它的用户，不会继承目录权限，如果需要新创建的文件继承目录的权限，可以使用 `setfacl` 命令。

:::tips
如果没有 `setfacl` 命令，可以安装 `acl` 软件包。
:::

## 选项参数

```bash
setfacl <options> <file>
```

```
-m, --modify=acl            修改当前文件的 ACL
-M, --modify-file=file      从文件中读取要修改的 ACL 规则
-x, --remove=acl            从文件的 ACL 中删除规则
-X, --remove-file=file      从文件中读取要删除的 ACL 规则
-b, --remove-all            删除所有扩展的 ACL 规则
-k, --remove-default        删除默认 ACL 规则
    --set=acl               设置文件的 ACL 规则，并替换当前的规则
    --mask                  重新计算有效权限
-n, --no-mask               不重新计算有效权限
-d, --default               设置默认 ACL 规则
-R, --recursive             递归对所有子目录/文件进行操作
-L, --logical               跟随符号链接
-P, --physical              跳过随符号链接
    --restore=file          从文件恢复 ACL 规则
    --test                  测试模式，不改变文件的 ACL 规则，列出操作后的规则
```

ACL 格式：

- `u[user]:uid[:perms]`
- `g[group]:gid[:perms]`
- `o[ther][:perms]`

要查看 ACL 规则，可以使用 `getfacl` 命令：

```bash
getfacl share
```

:::tips
使用 `-M` 和 `-X` 选项时，`setfacl` 接受 `getfacl` 生成的输出。
:::

## 示例用法

```bash
setfacl -m u:user1:rwx share
setfacl -d -m u:user1:rwx share
```

效果：

- `user1` 用户对 `share` 目录有 `rwx` 权限
- 对于任意用户在 `share` 目录创建的文件，`user1` 用户拥有 `rwx` 权限

## 使用场景

假设有一个项目的模块需要使用 `root` 权限调试，它会在特定目录产生一些文件，这些文件需要被其他模块(非 `root` 权限运行)读写，这种情况下可以通过 `setfacl` 命令使特定目录中新创建的目录/文件继承 ACL 规则。
