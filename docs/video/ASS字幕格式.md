# ASS 字幕格式

ASS 字幕示例：

```
[Script Info]
Title: [2001]Spirited.Away
ScriptType: v4.00+
ScaledBorderAndShadow: Yes
Synch Point: 0
PlayResX: 1920
PlayResY: 1080
Timer: 100.0000
WrapStyle: 0

[V4+ Styles]
Format: Name, Fontname, Fontsize, PrimaryColour, SecondaryColour, OutlineColour, BackColour, Bold, Italic, Underline, StrikeOut, ScaleX, ScaleY, Spacing, Angle, BorderStyle, Outline, Shadow, Alignment, MarginL, MarginR, MarginV, Encoding
Style: Default,微软雅黑,24,&H00FFFFFF,&H00000000,&H00000000,&H00000000,0,0,0,0,100,100,1,0,1,1,1,2,20,20,20,1

[Events]
Format: Layer, Start, End, Style, Name, MarginL, MarginR, MarginV, Effect, Text
;注释，对话字幕开始
Dialogue: 0,0:00:13.85,0:00:17.40,Default,,0,0,0,,(千寻 要保重哦 我们还会再见面的 理砂)
```

## [Script Info]

`ScriptType`

样式版本

`Synch Point`

可选，从哪一个时间点开始进行字幕加载播放

`PlayResX`，`PlayResY`

视频尺寸，下面的字号、边距、边框、阴影的大小基于此数值，建议和视频的比例一致，或者直接设置为视频的分辨率。

`Timer`

字幕加载的速度

`WrapStyle`

字幕超出屏幕宽度时的换行方式：

- `0` 智能换行，上面的行较长
- `1` 一行结束后从行尾的词换行
- `2` 不换行，只能使用 `\n`、`\N` 换行
- `3` 智能换行，下面的行较长

`ScaledBorderAndShadow`

边框宽度和阴影深度是否随着视频分辨率等比例缩放

- `No` 按照指定的像素显示(基于视频原始分辨率)
- `Yes` 随着实际分辨率缩放

## [V4+ Styles]

第一行是属性字段，之后的每一行为样式的属性值，每个属性之间用逗号分隔。

- `Name` 样式名称，在 `[Events]` 中引用
- `Fontname` 字体名称
- `Fontsize` 字体大小
- `PrimaryColour` 主要颜色，颜色格式为 `&HAABBGGRR`，十六进制，Alpha、蓝、绿、红
- `SecondaryColour` 次要颜色，卡拉 OK 效果使用
- `OutlineColour` 边框颜色
- `Backcolour` 阴影颜色
- `Bold` 粗体，`0` 关闭，`1` 开启，需要字体支持
- `Italic` 斜体，需要字体支持
- `Underline` 下划线
- `Strikeout` 删除线
- `ScaleX` 横向缩放，单位是 `%`，默认 `100`
- `ScaleY` 纵向缩放
- `Spacing` 字间距
- `Angle` 旋转，`0` 到 `360` 的角度值，正值为逆时针，负值为顺时针
- `BorderStyle` 边框样式，`1` 边框 + 阴影，`3` 纯色背景(文字下方为轮廓颜色背景，最下方为阴影颜色背景)
- `Outline` 边框宽度
- `Shadow` 阴影距离
- `Alignment` 对齐方式(字幕在屏幕中的位置)，取值 `1` 到 `9`，对应小键盘布局
- `MarginL` 字幕左边距，右对齐时无效
- `MarginR` 字幕右边距，左对齐时无效
- `MarginV` 字幕垂直边距，下对齐表示到底部的距离，上对齐表示到顶部的距离
- `Encoding` 字符编码，`1` 为默认，`0` 为 ANSI，`134` 为简体中文，`136` 为繁体中文，当字幕文件编码为 `UTF-8` 时该属性不起作用

## [Events]

字幕出现的时间和效果在该部分定义。

第一行是事件的属性字段，之后的为事件行，每个属性之间用逗号分隔。

```
[Events]
Format: Layer, Start, End, Style, Name, MarginL, MarginR, MarginV, Effect, Text
Dialogue: 0,0:00:13.85,0:00:17.40,Default,,0,0,0,,(千寻 要保重哦 我们还会再见面的 理砂)
```

### 格式字段

- `Layer` 整数，不同数值的字幕在重叠检测中被忽略，大数值的图层会覆盖在小的上面
- `Start` 开始时间，格式为 `小时:分:秒.毫秒`，小时只有一位，毫秒的取值是 `00` 到 `99`，最小单位是 `0.01` 秒
- `End` 结束时间，格式同上
- `Style` 样式名，如果引用的样式在 `[V4+ Styles]` 中不存在，那么就使用 `Default` 样式，如果 `Default` 也没有定义，引用渲染器自带的样式
- `Name` 角色名，这条字幕的角色名，对渲染没有意义，仅在编辑和设定时间轴时方便辨认
- `MarginL` 4 位左边距覆写值，`0` 表示使用 `Style` 中的值
- `MarginR` 4 位右边距覆写值，同上
- `MarginV` 4 位垂直边距复写值，同上
- `Effect` 过渡效果

### 样式覆写

`Dialogue` 中的 `Text` 可以覆写样式：

- 除了 `\h`、`\n` 和 `\N`，所有覆写代码在大括号 `{}` 内
- 覆写代码以反斜杠 `\` 开头
- 一个大括号内可以有多个覆写代码
- 覆写代码作用于其后的所有文本，可以使用 `{\r样式名}` 取消覆写代码，不写样式名则使用 `Default` 样式

常用覆写代码：

| 代码            | 说明                                                                            |
|---------------|-------------------------------------------------------------------------------|
| `\h`          | 硬空格，不换行，确保字幕不会在这个空格上分行，保证左右两个词在同一行                                            |
| `\n`          | 换行，如果启用了智能换行，则忽略此代码，示例：`怎么了?\n没事`<br>只在 `WrapStyle: 2` 或 `\q2` 时会换行，其他模式相当于空格 |
| `\N`          | 强制换行，`WrapStyle` 为任何模式都会换行                                                    |
| `\b<0/1>`     | `\b0` 正常，`\b1` 加粗，当参数大于 1 时，作为字体的 `weight`                                    |
| `\i<0/1>`     | `\i0` 正常，`\i1` 斜体                                                             |
| `\u<0/1>`     | `\u0` 正常，`\u1` 加下划线                                                           |
| `\s<0/1>`     | `\s0` 正常，`\s1` 加删除线                                                           |
| `\bord<宽度>`   | 设置边框宽度，单位为像素，可以为小数，有 `x` 或 `y` 时可单独设置 X/Y 轴宽度<br>示例：`\bord1`，`\xbord2`        |
| `\shad<深度>`   | 设置阴影深度，写法同上                                                                   |
| `\fn<字体名>`    | 指定一个系统中已安装的字体，区分大小写，如果指定的字体不存在，使用 `Arial` 字体<br>示例：`\fn思源黑体 Normal`           |
| `\fs<字体尺寸>`   | 指定字体尺寸                                                                        |
| `\fsp<像素值>`   | 指定文字间距                                                                        |
| `\a<位置>`      | 指定字幕在屏幕中的位置<br>1、2、3 是底部的左、中、右<br>5、6、7 是顶部的左、中、右<br>9、10、11 是中部的左、中、右        |
| `\an<位置>`     | 与小键盘布局相同，如果出现多个 `\a` 或 `\an` 以第一个为准                                           |
| `\fad(t1,t2)` | 简单的淡入淡出效果，`t1`、`t2` 分别为淡入、淡出时间长度，单位是毫秒                                        |