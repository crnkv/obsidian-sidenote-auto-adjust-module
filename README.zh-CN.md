# Obsidian 样式：用于边注（sidenote）的自适应模块


**用一个模块来提升其他边注（sidenote） CSS 代码片段的体验。**


## 功能介绍

实现边注（sidenote）功能的 CSS 代码片段通常都写死了边注的宽度和间隔距离。本模块 `Sidenote Auto-Adjust Module.css` (SAAM) 则可以根据窗口可见区域的大小自动计算适合的宽度数值，记录在 CSS 变量 `--sidenote-width` 、 `--sidenote-margin` 和 `--sidenote-edgeout` 里面，方便其他实现边注（sidenote）功能的代码片段调用。

这些变量会考虑以下情况而自动适应：

- “缩减栏宽”选项的开启与关闭（关闭时文档正文会占满可见区域，开启时文档行宽度会缩到 700px ）
- “显示行号”选项的开启与关闭（开启时行号栏会占用空间）
- 在使用的是默认主题还是 Minimal 主题（后者会将文档行宽度改成 40rem ）
- 当使用 Minimal 主题时，当前文档属性中，是否设定了 Minimal 主题特别设计的 'max' 类（cssclasses）（如添加了该属性， Minimal 主题会将文档行宽度改成 88% ）
- 当使用 Minimal 主题时，当前文档属性中，是否设定了 Minimal 主题特别设计的 'wide' 类（cssclasses）（如添加了该属性， Minimal 主题会将文档行宽度改成 50rem ）
- 当前文档属性中，是否设定了本 SAAM 模块特别设计的 'leftsided' / 'rightsided' 类（cssclasses）（如添加了该属性，本模块会将文档正文向左/右推移，留出右/左方更多的空间给边注）

本 SAAM 模块根据文档正文以外的可用空间的大小，选择以下之一的做法：

- 先尝试将 `--sidenote-width` 设置成文档行宽度的一半（例外是当文档行宽度是 50rem 时，先尝试设置成 20rem ，因为大部分人屏幕分辨率都不够塞下 50rem 加左右两边各 25rem ）
- 如果该设置会让边注（sidenote）有部分超出可见区域外，那就尝试设成该基础值的 75%
- 如果该设置还是不行，那就尝试设成 50% （即整个文档行宽度的 1/4 ）
- 如果可用空间比这还小，那就改成侵入性排版，让边注（sidenote）的外边缘超出文档正文边缘 `--sidenote-edgeout` ，并给边注设置一个合适的宽度上限且不超过文档行宽度的一半（文档正文会自动排版躲避重叠的）

## 使用方法

- 对于开发者，如果想将边注自适应模块的功能加到自己写的边注（sidenote）样式上，参考 `Sidenote Auto-Adjust Module showcase-template.css` ，里面分成三部分：边注的外观样式部分，边注的定位部分，和 fallback 设置值部分（最后是为了保证写出来的样式可以独立于本模块单独生效，只是不能自适应宽度而已）
- 对于一般用户，请将 `Sidenote Auto-Adjust Module.css` 和 `Sidenote in Tufte-style using SAAM.css` 或 `Stickynote in Callout-style using SAAM.css` 一起使用，后两者是两种常见的边注风格，详见下文。

### Tufte-style Sidenotes

本风格的灵感来自于 [Tufte-CSS](https://edwardtufte.github.io/tufte-css/) ，只不过 Tufte 将 "sidenote" 和 "marginnote" 分成两种边注（前者用纯 CSS 实现自动编号，后者则不加编号）。

本代码片段 `Sidenote in Tufte-style using SAAM.css` 简化设计，采用“编号应当显式手写”的理念，毕竟这也是为了保证阅读 markdown 原始文本的时候也能有相同的阅读体验，减少隐式。

使用本代码片段，可以用以下两种语法：

- Inline 语法（就一 HTML 标签，加上 "sidenote" 类）
```
<span class=sidenote> text here </span>
```
- Block 语法（是对 obsidian Callout 语法的 hack ，使用特殊的 "sidenote" callout-type ，灵感来自于 [FireIsGood](https://github.com/r-u-s-h-i-k-e-s-h/Obsidian-CSS-Snippets/blob/Collection/Snippets/Sidenote%20callout%2002.md)）
```
> [!sidenote]
> text here
```

用例：

- Inline 语法
```
May the force be with you<sup>[1]</sup><span class=sidenote> $^1$ See [[Author]], _Book Name_. <br/>![](https://url/to/image.jpg) </span>
```
- Block 语法
```
> [!sidenote]
> $^1$ See [[Author]], _Book Name_.
> ![](https://url/to/image.jpg)
> 
> - 列表条目
>
> | 表格单元格 | 表格单元格 |
> | --------- | --------- |
> | 表格单元格 | 表格单元格 |
（空行）
May the force be with you<sup>[1]</sup>
```

两种语法都能生成极简主义的、小字体、无背景边注（sidenote），在实时预览模式和阅读模式都能用，但是 inline 语法有如下几个限制：

1. inline 语法要用 `<br/>` 来换行，并且无法容纳多行的 markdown 特性，如表格、列表、代码块等等，并且
2. 它里面的 markdown 语法只在阅读模式中渲染（在实时预览模式只显示成 markdown 源代码）。

不过 inline 语法的好处是它生成的边注（sidenote）会和引用它的正文锚点保持在同一行，能精准定位，而不像 block 语法生成的边注只能和段落的首行对齐。

从 markdown 原始文本的可读性角度看， inline 语法不引入非 markdown 的语法，锚点和边注相邻，给别人看也有很高的可读性，而 block 语法则使用了 callout ，并且还是对 callout 的 hack ，从别人看来正常来说只会理解成这是某种 callout 。

两种语法都各自有靠右、靠左、默认的三种变体：

- `<span class=sidenote-r>`,  `<span class=sidenote-l>`,  `<span class=sidenote>`
- `>[!sidenote-r]`,  `>[!sidenote-l]`,  `>[!sidenote]`

默认位置可以通过 Style Settings 插件设置成全局的靠右或靠左，而对于每一个文档，可以通过给文档属性添加本模块设计的 'leftsided' / 'rightsided' 类（cssclasses），来将文档正文向左/右推移，并同时将边注的默认位置改为靠右/靠左。

### Callout-style Stickynotes

本风格的灵感来自于 [sailKite](https://github.com/r-u-s-h-i-k-e-s-h/Obsidian-CSS-Snippets/blob/Collection/Snippets/Sidenote%20callout%2001.md) 和 [xhuajin](https://github.com/xhuajin/obsidian-sidenote-callout)。

本风格是对 obsidian Callout 语法的 hack ，并且被设计为保留了 Callout 的大部分外观风格，例如颜色背景、 callout 标题等，这使得它看上去更像是拟物风的便利贴（stickynote），而不仅仅是一小段边注（sidenote）文字。

使用本代码片段，可以用以下两种语法：

- 简化的便利贴（stickynote）风格（使用特殊的 "stickynote" callout-type）
```
> [!stickynote]
> text here
```
- 完整的 callout 风格（使用特殊的 "aside" callout-metadata）
```
> [!info|aside] (可选标题、折叠标记)
> text here
```

用例：

- 简化的便利贴（stickynote）风格
```
> [!stickynote]
> $^1$ See [[Author]], _Book Name_.
（空行）
May the force be with you<sup>[1]</sup>
```
- 完整的 callout 风格
```
> [!warning|aside]- 标题：这是折叠注释，点击展开
> This is simply a warning
> - 列表条目 1
> - 列表条目 2
```

两种语法都能容纳多行的 markdown 特性，且在实时预览模式和阅读模式都能用。两者的区别是，简化的便利贴（stickynote）风格隐藏了 callout 标题元素，并且调成了紧凑的行距，而完整的 callout 风格则会占用更多空间，并且可适用于各种 callout-type 。

两种语法都各自有靠右、靠左、默认的三种变体：

- `>[!stickynote-r]`,  `>[!stickynote-l]`,  `>[!stickynote]`
- `>[!xxxx|aside-r]`,  `>[!xxxx|aside-l]`,  `>[!xxxx|aside]`

默认位置可以通过 Style Settings 插件设置成全局的靠右或靠左，而对于每一个文档，可以通过给文档属性添加本模块设计的 'leftsided' / 'rightsided' 类（cssclasses），来将文档正文向左/右推移，并同时将边注的默认位置改为靠右/靠左。

## 快速插入边注

在 `templates for quick insert` 路径下，提供了四个模板文件，可以和 Templater 插件一起用，以便在光标位置插入边注（sidenote）或便利贴（stickynote）。还可以为这些模板设置快捷键。灵感来自于 [鱼先生的模块化Obsidian](https://www.bilibili.com/video/BV1zj6iYuEMp)。

别忘了将 Templater 插件的 'Automatic jump to cursor' 选项打开。

## Style Settings

使用 [Style Settings](https://github.com/mgmeyers/obsidian-style-settings) 插件，你可以不用动代码就能修改以下全局设置：

- 边注的默认位置（靠左/靠右）
- 边注与文档正文的间隔距离
- 当“缩减栏宽”选项关闭时（文档正文占满可见区域），侵入式排版允许让边注挤占正文区域的空间大小
- 当“缩减栏宽”选项开启、且使用默认主题时，侵入式排版允许让边注挤占正文区域的空间大小
- 当“缩减栏宽”选项开启、且使用 Minimal 主题时，侵入式排版允许让边注挤占正文区域的空间大小
- 当“缩减栏宽”选项开启、且使用 Minimal 主题、且当前文档属性添加了 'wide' 类时，侵入式排版允许让边注挤占正文区域的空间大小
- 当“缩减栏宽”选项开启、且使用 Minimal 主题、且当前文档属性添加了 'max' 类时，侵入式排版允许让边注挤占正文区域的空间大小
