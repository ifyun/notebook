# ufw 配置防火墙

Ubuntu 自带的防火墙配置工具为 `ufw`，它用于管理 `iptables` 防火墙规则，使用起来非常简单。

## 安装/检查 UFW 的状态

以下命令可检查状态：

```bash
sudo ufw status verbose
```

输出如下：

```bash
Status: inactive
```

如果 `ufw` 已经激活则显示为 active，同时会输出已经开放的端口。可以使用以下命令开启：

```bash
sudo ufw enable
```

> 使用 `disable` 禁用，`reset` 可重置。

## 开放端口

有两种方式来开放端口，一种是直接使用服务名：

```bash
sudo ufw allow http     # 开放 80 端口
sudo ufw allow ssh      # 开放 22 端口
```

也可以指定端口和协议：

```bash
sudo ufw allow 8080/tcp
```

> 若没有指定协议，将同时创建 TCP 和 UDP 规则。

UFW 也支持使用端口范围：

```bash
sudo ufw allow 5000:6000/tcp
```

## 允许 IP 和子网

允许指定的 IP 访问，使用 `from` 关键字：

```bash
sudo ufw allow from 172.16.1.100
```

允许指定的 IP 访问指定的端口：

```bash
sudo ufw allow from 172.16.1.100 to any port 22
```

指定子网：

```bash
sudo ufw allow from 172.16.1.0/24 to any port 3306
```

> 可以指定多个端口，用逗号隔开即可。

## 禁止连接

禁止连接使用 `deny` 关键字替换 `allow`，例如服务器开放了 http 和 https 端口，要禁止特定的 IP 或子网连接该端口：

```bash
sudo ufw deny from 172.16.1.0/24 to any port 80,443
```

若不指定端口，则拒绝该 IP 或子网连接到任何端口。

## 删除规则

### 1. 通过序号删除

运行一下命令：

```bash
sudo ufw status numbered
```

输出：

```text
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 2375/tcp                   ALLOW IN    Anywhere                  
[ 2] 22/tcp                     ALLOW IN    Anywhere                  
[ 3] 2375/tcp (v6)              ALLOW IN    Anywhere (v6)             
[ 4] 22/tcp (v6)                ALLOW IN    Anywhere (v6)  
```

使用以下命令即可删除指定的规则：

```bash
sudo ufw delete 3
```

### 2. 指定端口删除

假如开放了 3306 端口，可以使用一下命令删除：

```
sudo ufw delete allow 3306
```
