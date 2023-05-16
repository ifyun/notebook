# printk 打印级别

在 Linux 内核中，`/proc/sys/kernel/printk` 文件用于设置打印信息的日志级别。

```bash
cat /proc/sys/kernel/printk
```

其中包含了 4 个整数值，分别代表：

1. 控制台日志级别，优先级高于该值的消息将被打印至控制台
2. 默认的消息日志级别，没有优先级的消息将使用该值
3. 最低的控制台日志级别，日志级别可被设置的最小值(最高优先级)
4. 默认的控制台日志级别

这 4 个值在 `kernel/printk.c` 中定义：

```c
int console_printk[4] = {
    DEFAULT_CONSOLE_LOGLEVEL,     /* console_loglevel */
    DEFAULT_MESSAGE_LOGLEVEL,     /* default_message_loglevel */
    MINIMUM_CONSOLE_LOGLEVEL,     /* minimum_console_loglevel */
    DEFAULT_CONSOLE_LOGLEVEL,     /* default_console_loglevel */
};
```

日志级别的取值为 0 ~ 7 一共 8 个级别：

- 0 - `KERN_EMERG`
- 1 - `KERN_ALERT`
- 2 - `KERN_CRIT`
- 3 - `KERN_ERR`
- 4 - `KERN_WARNING`
- 5 - `KERN_NOTICE`
- 6 - `KERN_INFO`
- 7 - `KERN_DEBUG`

!> 数值越小，级别越高。

可使用以下方式实时修改级别：

```bash
sudo bash -c "echo 7 4 1 7 > /proc/sys/kernel/printk"
```

修改 `/proc/sys/kernel/printk` 的内容在重启后会恢复。若需要永久生效，编辑 `/etc/sysctl.conf` 文件：

```
# Uncomment the following to stop low-level messages on console
kernel.printk = 3 4 1 3
```

?> 若系统启动并登录后，一直有日志输出到屏幕，可以将第一个值设置到 `KERN_DEBUG` 以上。
