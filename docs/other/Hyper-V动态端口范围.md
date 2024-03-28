# Hyper-V 动态端口范围

在使用 Hyper-V、WSL2、Docker Desktop 的时候，会出现某些端口无法绑定的问题，例如：

- Vite 开发服务器无法绑定 5173 端口
- Docker 运行 MySQL 映射 3306 失败

这是 Hyper-V 的默认交换机在作怪，它通过 NAT 来共享主机网络，动态端口范围默认从 1024 开始。

## 修改端口范围

可以使用以下命令查看端口范围：

```bash
netsh int ipv4 show dynamicport tcp
```

要解决这个问题，只需要将端口起始值设置在常用端口以上即可：

```bash
netsh int ipv4 set dynamicport tcp start=16384 num=16384
```

运行以下命令立即生效：

```bash
net stop winnat
net start winnat
```

## 例外端口

可以将某个范围的端口设置为例外，不准 NAT 使用：

```bash
netsh int ipv4 add excludedportrange protocol=tcp startport=8080 numberofports=1
```

查看例外端口：

```bash
netsh interface ipv4 show excludedportrange protocol=tcp
```
