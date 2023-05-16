# 使用 fstab 自动挂载分区

新增加的硬盘或分区 Linux 不会自动挂载，需要在 `/etc/fstab` 文件中配置。

> 请先格式化新硬盘或分区。

## 找到要挂载的分区

两种方式：

1.使用分区的 UUID：

```bash
sudo blkid
```

找到要挂载的分区的 UUID，记下备用。

2.使用分区路径：

```bash
sudo fdisk -l
```

找到要挂载的分区路径，例如：`/dev/sdb1`

## 创建挂载点

```bash
sudo mkdir /mnt/data
```

## 配置 fstab

编辑 `/etc/fstab`：

```bash
sudo vim /etc/fstab
```

加入内容：

```
UUID=<第一步找到的UUID>  <挂载点>  <分区类型>  defaults  0  2
```

示例:

```
UUID=e3874314-cb82-334e-84b8-12c2f91796e7  /mnt/data  ext4  defaults 0 2
```

使用分区路径：

```
/dev/sdb1   /mnt/data  ext4  defaults 0 2
```

### fstab 各列参数说明

| 列 | 参数         | 说明                                |
|---|------------|-----------------------------------|
| 1 | fs_spec    | 分区定位、可以是 UUID 或 LABEL（如果有）        |
| 2 | fs_file    | 挂载点位置，例如：`/mnt/data`              |
| 3 | fs_type    | 挂载的磁盘类型，格式化成什么类型就填什么类型            |
| 4 | fs_options | 挂载参数，一般为 `defaults`               |
| 5 | fs_dump    | 能否被 dump 备份，0 表示不做备份              |
| 6 | fs pass    | 是否检查扇区，0->不检查，1->最早检查，2->1级别完成后检查 |

## 验证配置是否正确

```bash
sudo mount -a
```

!> 如果搞挂了，可用 Live CD 来修改。
