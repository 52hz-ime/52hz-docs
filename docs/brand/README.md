# 独鲸 · 品牌素材

<img src="whale-icon.svg" alt="52Hz 方形独鲸图标" width="64" height="64">

[打开独鲸动画](whale-silhouette.html)。这个可离线打开的单文件页面是造型和动效的参考：默认是纯剪影，右下角的「鲸腹」开关可显示唯一的第二色块，切换不打断游动。

| 素材 | 用途 |
| --- | --- |
| [透明鲸标 SVG](whale-mark.svg) | 取自动画的滑翔姿态。用于浅色页眉、页脚或空状态插图；保持原比例。 |
| [透明鲸标 SVG · 鲸腹版](whale-mark-belly.svg) | 与单色版同轮廓、同画幅，增加动画中的蓝灰色鲸腹。可直接替换单色版，用于较大的页面插图。 |
| [方形图标 SVG](whale-icon.svg) | 用于 favicon、应用入口和头像。自带雾蓝底，小尺寸下略加粗尾柄，保留分叉尾鳍。 |
| [章节分隔线 CSS](whale-divider.css) | 用于浅色长文页面。两条细线在同一枚透明鲸标两侧留白，不引入另一种鲸形。 |

## 使用示例

以下路径相对于品牌目录。复用分隔线时，将样式表与 `whale-mark.svg` 放在同一目录。

```html
<link rel="icon" type="image/svg+xml" href="whale-icon.svg">
<link rel="stylesheet" href="whale-divider.css">

<img src="whale-mark.svg" width="96" alt="52Hz">
<hr class="whale-divider">
```

## 造型约定

- 保留向右的圆头、水平舒展的尾柄、分叉的尾鳍和垂下的胸鳍；不用眼睛、腮红、腹纹或描边传达情绪。
- 默认用深靛蓝 `#183447` 单色。小图标始终用纯剪影；大幅插画可选用蓝灰 `#607D89` 表示鲸腹。
- 用留白和尺度表现独游。需要动效时参考定稿页：划尾后滑翔，背景向后移动；系统偏好减少动态时保留静帧。
