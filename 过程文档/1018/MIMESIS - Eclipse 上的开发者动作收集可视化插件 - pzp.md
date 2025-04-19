# MIMESIS: Eclipse 上的开发者动作收集可视化插件

[Recording, Visualising and Understanding Developer Programming Behaviour](https://ieeexplore.ieee.org/abstract/document/9425961)

这篇论文是 IEEE21 的一篇文章，是 ICSE'24 IDE Workshop 论文 Understanding and Evaluating Developer Behaviour in Programming Tasks 的前身，主要介绍能收集开发者行为并可视化的 Eclipse 插件 MIMESIS。

## 摘要

要理解开发人员是如何解决编程任务的，有必要观察他们在做什么，即，他们执行什么特定的动作，他们应用哪些策略，以及他们如何利用可能存在的，但尚未知道的或尚未可用的信息来解决任务。为此，**我们为Eclipse IDE实现了一个插件，它可以捕获开发人员在处理编程任务时与IDE执行的几乎所有交互**。这使我们能够全面地跟踪开发人员的行为，例如，是否以及何时执行了解决给定任务所需的代码编辑，甚至更有趣的是，导致开发人员这样做的前面步骤是什么。在使用新插件进行的第一次实验中，我们能够观察到操作模式和程序理解阶段，它们证实了以前的研究结果，并且直到最近的文献才部分地怀疑这些结果，但以前从未真正观察到。

> 该工具的视频介绍：https://youtu.be/GeZI-vCdgfo

## 相关研究

本文提到了FEEDBAG：

​		Proksch等人使用他们的工具FEEDBAG，一个VISUAL STUDIO的“通用交互跟踪器”来记录开发人员的交互。为此，他们定义了“丰富事件流”，这是开发活动期间发生的IDE事件的元模型。他们还引入了对事件的分类，这启发了我们对开发任务中可能发生的事件进行分类和分组的方法。虽然Proksch等人专注于展示他们的事件数据模型和相应的记录概念，但没有提供具体的结果，也没有对他们迄今记录的交互数据进行具体的评估或可视化，但其他作者使用了他们的FEEDBAG工具集收集的数据进行评估。

之后本文作者对该插件收集到的数据进行分析，用于评估开发人员在解决编程任务时的不同表现，撰写了论文Understanding and Evaluating Developer Behaviour in Programming Tasks。

## MIMESIS介绍

> 作者称MIMESIS是开源的，提供的链接为 https://dfgpc.informatik.uni-bremen.de/mimesis/，但是其中“下载”部分是经典的“Coming soon”。
>
> 找到了另外一个链接 https://gitlab.informatik.uni-bremen.de/dfgpc/mimesis

MIMESIS允许记录开发人员与Eclipse IDE的交互，并将这些捕获的交互可视化，以供进一步检查。此外，它还提供了一个接口，允许其他应用程序使用记录的交互和事件数据，例如，用于推荐工具。

当在ECLIPSE会话中使用时，MIMESIS插件记录了开发人员与IDE执行的几乎所有交互。捕获尽可能多的交互是很重要的，因为每个活动都可能表明开发人员的任务完成中的某个状态。如果一个开发人员在一段时间内没有执行一个明显的动作，如编辑代码，这可以被认为是开发人员离开办公桌的迹象，另一方面，也可能发生开发人员只是非常仔细地检查屏幕上显示的代码。

## 开发事件分类

### 活动

一般活动的类别，例如，与开发活动不直接相关的活动

- RecordingEvent -记录开始/停止
- ScrollEvent -用户在编辑器窗口内滚动
- TextSelectionEvent -文本被选中

### 执行

涉及运行程序的事件

- DebugEvent -调试相关事件，例如，设置或命中断点
- LaunchEvent -项目被执行或调试
- WebBrowserEvent -内部浏览器启动，访问的url和搜索查询被记录

### 编辑

与源代码更改和注释相关的事件

- CodeChangeEvent - IDE中的源代码被修改
- CodeCompletionEvent -使用了代码补全
- TextCommentEvent -开发者添加了一个文本注释（在MIMESIS插件中）
- VoiceCommentEvent -添加了一个音频注释（MIMESIS插件中的一个特殊功能）

### 环境

改变IDE外观的事件，如改变IDE的主窗口

- EditorEvent -与编辑器窗口相关的特定事件，例如，调整或移动、
- PerspectiveEvent -视角改变/打开
- ViewEvent -视图打开/关闭，或者IDE中的焦点发生了变化
- WindowEvent -IDE的窗口焦点发生了变化，例如，选择了不同的窗口

### 导航

与开发人员导航相关的事件

- EditorMouseEvent -鼠标动作，如点击或移动鼠标
- EditorTextCursorEvent -文本光标被移动
- SearchEvent -被执行的搜索操作
- TreeSelectionEvent -树视图中的元素被选中
- TreeViewerEvent -导航视图中的元素被折叠/展开

### 解决方案

与解决方案相关的事件，应用程序项目的Eclipse名称

- FileEvent -与单个文件相关的事件
- ProjectEvent -项目已被加载或卸载
- ResourceEvent -文件被创建、删除或更改
- SaveEvent -文件被保存

## 可视化

为了进一步分析记录的相互作用，记录的数据被转换成视觉表示。使用一种“空间”代码表示，这将使我们能够实际“看到”开发人员在源代码、文档和其他信息之间的遍历。例如，相应文件中交互的行号位置绘制在x轴上，而y轴被选择用来表示记录数据的时间顺序，与视频编辑或类似的合成软件中“轨道”的视觉呈现方式有些相似。