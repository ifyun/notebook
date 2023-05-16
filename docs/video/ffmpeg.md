# FFmpeg 常用命令

## 查看文件信息

```bash
ffmpeg -i input.mkv
```

将输出各个轨道的信息：章节、视频编码、音频编码、字幕类型等等。

## 提取轨道

### 单个视频/音频轨

提取视频轨道：

```bash
ffmpeg -i input.mkv -an -vcodec copy output.mp4
```

提取音频轨道：

```bash
ffmpeg -i input.mkv -acodec copy -vn output.mlp
```

- `-an` 表示不处理音频
- `-vn` 表示不处理视频

> 对 mp4 文件也有效。

### 多个视频/音频/字幕轨

对个轨道需要使用 `-map` 选项提取：

```bash
ffmpeg -i input.mkv -an -vcodec copy -map 0:v:0 video_track_0.mp4
```

以上命令提取 `input.mkv` 中的第一个视频轨。

- 第一个 `0` 表示第一个输入文件 `input.mkv`
- 第二个 `v` 表示视频轨
- 第三个 `0` 表示第一个视频轨

提取第一个音频轨：

```bash
ffmpeg -i input.mkv -vn -acodec copy -map 0:a:0 audio_track_0.mlp
```

提取第一个字幕轨：

```bash
ffmpeg -i input.mkv -map 0:s:0 sub_track_0.ass
```

!> 如果字幕是其他类型请改为对应的扩展名。