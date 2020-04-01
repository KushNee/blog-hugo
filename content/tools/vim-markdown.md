+++
title = "在 vim 中快速书写 markdown 需要的插件"
date = 2020-04-01T09:12:42+08:00
draft = false
tags = ["vim", "markdown"]
categories = ["Tools"]
isCJKLanguage = true
author = "Kush Nee"
+++

什么是最好的 markdown 编辑器呢？vscode、typora、马克飞象、mweb。。。
市面上有无数的编辑器可以用来进行 markdown 编辑。但是目前看来，灵活度最高的还是只有 vim 和 emacs。

我曾经有一段时间用过 spacemacs，不得不说，强行 hybried 最为致命。我没学到太多的 emacs 知识，反而是过去熟悉的 vim 慢慢地开始遗忘。
所以最后，我还是返回了 vanilla vim 的行列。

当然不能说是完全的纯 vim，而是 neovim。一方面是 neovim 在 floating window 方面的成果比较显著，而浮动窗口是能有效减少打断注意力的。
而我要说的 markdown 相关插件中有一个需要以 neovim 为主要版本。所以，就是这么回事了。

<!--more-->
---

## [markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim)

markdown 预览插件。这个就是我说的使用 neovim 的一大动力。作者之前写了 vim 版本的，但是去年就结束支持了。而 neovim 版本提供了开箱即用的 katex 支持和 Plantuml 支持等。
更多功能，更活跃的开发，很好的选择。

## [vim-markdown](https://github.com/plasticboy/vim-markdown)

看名字也知道是一个希望大包大揽的项目。它支持折叠、显示 Toc、高亮代码块、高亮公式。。。以及其他很多小功能。要类比的话就像 vscode 上的 markdownallinone。

高亮基本是默认开启的，不过如果你想要高亮 latex 公式的话，需要在配置里添加 `let g:vim_markdown_math = 1`。除此之外，没有什么需要配置的了。

## [vim-table-mode](https://github.com/dhruvasagar/vim-table-mode)

vim-markdown 的安装说明里，会要求你先安装一个插件 [tabular](https://github.com/godlygeek/tabular)，实际上就是为了完成表格格式化的。
写完表格执行 `:TableFormat` 可以完成相关线条和元素的对齐。

那么如果你想要在写的时候就直接完成对齐呢？那么你可以使用另一个插件，vim-table-mode。进入 markdown 文件后，执行 `:TableModeToggle` 或者 `:TableModeEnable` 来开启。

开启后，每一行写完都会触发格式化，书写会工整很多。

## [bullets](https://github.com/dkarter/bullets.vim)

markdown 中列表肯定是必不可少的。列表一般有这么几个问题：

1. 层次切换。也就是 1 级变 2 级之类的操作。
2. 对已存在的有序列表进行插入后序号出错。

这两个,bullets 都能解决。使用 `<Ctrl-t>` 来进位（类似其他编辑器的 `tab` 按键），使用 `<Ctrl-d>` 退位（类似 `<shift-tab>` 按键）。
而对于有序列表的重排序，你可以在选中多行后，执行 `:RenumberSelection` 自动重排。非常方便。

### NOTE

命令的键入比较麻烦，建议可以把这些命令统一绑定到 localleader 上，[which-key](https://github.com/liuchengxu/vim-which-key) 插件可以很清楚得展示这些按键信息。
