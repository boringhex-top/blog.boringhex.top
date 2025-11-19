---
title: 分享一个Gerber工具
date: 2024-11-29 11:24:53
tags:
    - eda
    - 工具
    - 资源
---

这个项目是几年前的，提供了一套操作Gerber文件的小工具，非常小巧好用，可以用来查看Gerber文件，也可以用来编辑Gerber文件，最近没有更新了。

<!-- more -->

仓库地址👉 https://github.com/ThisIsNotRocketScience/GerberTools

这个工具是用C#写的，我用的时候是在Windows上，不过作者说可以在Linux上用Mono运行，我没有试过。

软件安装目录如下：

```shell
├─BMPGerberMerger
├─CaseBuilder
├─CommandLineTools
├─FrameBuilder
├─Panelizer
│  └─Help
├─Project_Utilities
│  └─SVGTilings
│      ├─14Fold
│      └─HexaThing
├─QuickRender
└─Viewer
```

- **BMPGerberMerger**: 用于将BMP格式的图像与Gerber文件进行合并。Gerber文件通常用于描述印刷电路板（PCB）的图形信息，因此该工具可能在PCB设计和生产过程中用于将图像叠加到PCB设计中。
- **CaseBuilder**: 用于创建或设计外壳或盒子。可能用于生成PCB的外部保护壳或设备的机箱设计。
- **CommandLineTools**: 这个目录下包含了一系列命令行工具，这些工具用于自动化处理Gerber文件的各种任务，如转换、合并、或检查Gerber文件的完整性。
- **FrameBuilder**: 用于创建边框，可能是在PCB设计中用于定义板边界或添加结构支撑。
- **Panelizer**: 用于将多个PCB设计拼板，以便在生产过程中同时制造多个PCB。这在大规模生产中可以提高效率并降低成本。`Help`子目录包含该工具的使用文档或帮助文件。
- **Project_Utilities**: 这个目录包含各种项目相关的实用工具，用于管理和优化PCB项目。
  - **SVGTilings**: 这个子目录涉及SVG格式的平铺或图案填充。
  - **14Fold 和 HexaThing**: 这些子目录是特定的平铺或图案算法，可能用于设计中需要重复图案的部分。
- **QuickRender**: 用于快速渲染PCB设计的图形表示，以便于快速检查和验证设计。
- **Viewer**: 用于查看和浏览Gerber文件的工具，提供对PCB设计的可视化工具，以便于设计审查和验证。

在电子设计自动化（EDA）领域，Gerber格式是用于描述印刷电路板（PCB）设计的行业标准。Gerber文件包含制造PCB所需的所有信息，确保设计可以被准确地传输到制造阶段。这篇博客文章将详细介绍Gerber格式的历史、结构、用途以及如何创建和查看Gerber文件。

## Gerber格式的历史背景

Gerber格式最初由Gerber Systems公司（现为Ucamco）开发，旨在为光绘仪设备提供标准化输入。自创建以来，Gerber格式经历了多次演变，以适应PCB设计和制造技术的不断发展。当前使用最广泛的版本是扩展Gerber格式，也称为Gerber X2，支持多层板和元数据的传输。

## Gerber文件的结构

Gerber文件通常以RS-274X格式保存，是ASCII文本文件，包含以下主要部分：

- 元数据：包括文件的基本信息，如文件格式、单位、层次名称等。
- 图形原语：定义PCB的几何形状，包括线条、圆形、矩形和多边形等。
- 指令代码：用于控制绘图指令的代码，通常以“G”开头（例如，G01表示线性插补）。
- 闪光命令：用于在特定位置绘制焊盘或孔。

每个Gerber文件通常代表PCB设计的一个特定层，如铜层、阻焊层或丝印层。完整的PCB设计可能需要多个Gerber文件来描述所有的层次。

## Gerber格式的用途

Gerber文件在PCB制造过程中起着关键作用。它们用于：

- 定义电路图形：描述PCB上的导电路径和焊盘位置。
- 生产掩膜：用于制作阻焊层和丝印层。
- 钻孔信息：提供用于生产钻孔的精确位置和尺寸。

通过这些文件，PCB制造商可以将设计转化为实际的电路板。

## 如何创建和查看Gerber文件

### 创建Gerber文件

PCB设计软件（如Altium Designer、KiCad、EAGLE等）通常提供了生成Gerber文件的功能。创建过程通常包括以下步骤：

- 设计电路板：使用设计软件创建PCB布局。
- 生成Gerber文件：在设计软件中选择导出或生成Gerber文件选项。确保为每个PCB层生成单独的文件。
- 检查设置：验证设置是否正确，包括单位、格式和层的选择。

### 查看Gerber文件

查看Gerber文件可以使用专门的Gerber查看器软件，如GerbView、ViewMate或在线工具。查看过程包括：

- 加载文件：打开Gerber查看器软件并加载生成的Gerber文件。
- 检查各层：逐层查看PCB设计，确保所有细节准确无误。
- 验证设计：确认没有设计缺陷，如未连接的焊盘或过密的导线。

## Gerber格式的优势和挑战

### 优势

- 行业标准：被广泛接受和使用，确保设计和制造之间的无缝对接。
- 详细描述：能够精确描述复杂的PCB设计。
- 跨平台兼容：支持多种设计软件和制造设备。

### 挑战

- 学习曲线：理解Gerber格式要求一定的专业知识。
- 格式更新：随着技术进步，需要跟上格式的更新和新功能的引入。

访问GitHub不方便的同学可以后台发送 `600002` 获取软件包。