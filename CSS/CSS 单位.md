# CSS 单位

## 绝对长度

绝对长度单位是固定的，用任何一个绝对长度表示的长度都将恰好显示为这个尺寸。

不建议在屏幕上使用绝对长度单位，因为屏幕尺寸变化很大。但是，如果已知输出介质，则可以使用它们，例如用于打印布局（print layout）。

| 单位   | 描述                       |
| ---- | ------------------------ |
| cm   | 厘米                       |
| mm   | 毫米                       |
| in   | 英寸 (1in = 96px = 2.54cm) |
| px * | 像素 (1px = 1/96th of 1in) |
| pt   | 点 (1pt = 1/72 of 1in)    |

* 像素（px）是相对于观看设备的。对于低 dpi 的设备，1px 是显示器的一个设备像素（点）。对于打印机和高分辨率屏幕，1px 表示多个设备像素。

## 相对长度

相对长度单位规定相对于另一个长度属性的长度。相对长度单位在不同渲染介质之间缩放表现得更好。

| 单位   | 描述                                       |
| ---- | ---------------------------------------- |
| em   | 相对于元素的字体大小（font-size）（2em 表示当前字体大小的 2 倍） |
| ex   | 相对于当前字体的 x-height(极少使用)                  |
| ch   | 相对于 "0"（零）的宽度                            |
| rem  | 相对于根元素（html）的字体大小（font-size）             |
| vw   | viewpoint width，视窗宽度，1vw=视窗宽度的1%         |
| vh   | viewpoint height，视窗高度，1vh=视窗高度的1%        |
| vmin | vw和vh中较小的那个。                             |
| vmax | vw和vh中较大的那个。                             |
| %    | 相对于父元素的大小。                               |
