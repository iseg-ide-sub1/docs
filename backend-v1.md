# 后端-数据处理

主要五个模块：**预处理、模式识别、日志总结、意图预测、能力分析**

向上提供能力：日志总结、意图预测、能力分析

**计划先按这版方案把原型调通了，这版至少知道能怎么编写了，根据实际效果再看哪些位置需要调整/提升复杂度。**

[TOC]

## 预处理

插件前端收集数据时进行简单的合并处理，形成raw_data。在预处理模块进一步处理：

1. **基于粘度的操作合并**：不一定在这一步弄，宗旨是保证在模式识别的步长内装得下足够有用的信息，这个粘度其实和模式识别差不多，可以放到后面。
2. **event_type转换**：前端收集的事件类型基于API，能进一步总结
3. **剔除冗余数据**：看数据集情况
4. **形成历史操作件artifact_history**：用于后续的预测检索
5. **形成历史命令cmd_history**：用于后续的预测检索

## 模式识别

根据程序员开发过程中普遍的几种行为类型，初版原型工具设定如下几种固定的模式/状态，状态内的操作都有明显的类型倾向：

1. **配置环境**：执行的命令类型多为install、可能出现静态检查问题、操作的文件多为配置文件
2. **调查阅读**：鼠标滚轮滑动、选中文本但不编辑、文件跳转、代码符号跳转、搜索事件密集，编辑操作少
3. **编写内容**：一段时间内主要事件都是文本变化，可能包含少量的其他操作
4. **执行验证**：频繁执行命令和调试，可能配合少量编辑操作，原因复杂，这种情况可能是在调整运行效果、验证某些功能，也可能是在修复某些问题，直接原因可能来自调试输出或终端也可能不是，因此统称为“验证”
5. **未知**：不属于任何一类模式，单纯就是在IDE里但是什么都没做。你不能指望程序员无时无刻不知道自己该干什么，程序员自己都不知道该干什么时，我们更不知道。

| 方案                                                         | 输入                   | 输出 |
| ------------------------------------------------------------ | ---------------------- | ---- |
| CFC液态神经网络（offline）<br />预训练模型，分类固定，不随使用而更新<br />特征编码：[操作类型, 环境反馈类型, ref关系] | 预处理过的动作数据序列 | 模式 |

## 日志总结

提供日志总结功能，总结程序员最近在干什么事，干的内容是什么，具体包括：

1. **工作模式**：调用模式识别模块
2. **聚焦位置**：程序员当前关注的操作件或命令，来源于artifact_history和cmd_history

| 方案                                    | 输入                   | 输出          |
| --------------------------------------- | ---------------------- | ------------- |
| 聚焦位置直接选用频度最高的artifact和cmd | 预处理过的动作数据序列 | 模式+聚焦位置 |

## 意图预测

预测开发者的意图，具体包括：

1. **工作模式**：调用模式识别模块
2. **操作**：操作对象、命令。

其中后端计算模式，提供artifact_history、cmd_history中的检索结果，然后需要指导前端进一步根据repo内容获取每个工作模式下需要进一步提供的信息(不在操作历史中)。

#### 后端检索

规则+检索(先尝试简单的机器学习模型例如逻辑回归，online，增量式学习，实时更新)

**每个模式单独用一个模型，一共5个模型**

检索方法：

1. 对artifact_history中每个artifact打分
2. cmd_history的有关命令（频度统计+规则匹配）

其中对于输入特征中指标的计算，范围是属于该模式的最近操作历史（模式识别会给到有用的检索范围）

| 方案               | 输入                                                         | 输出 |
| ------------------ | ------------------------------------------------------------ | ---- |
| 对artifact打分排序 | artifact：[频度, 与其他artifact的ref距离, 与其他artifact的振荡耦合度] | 分数 |

#### 前端输出

| 模式     | 输入               | 输出                                                |
| -------- | ------------------ | --------------------------------------------------- |
| 配置环境 | 模式+检索结果+指导 | 模式+检索结果+仓库的配置文件等(规则匹配)            |
| 调查阅读 | 模式+检索结果+指导 | 模式+检索结果+与检索结果涉及的ref相关artifact       |
| 编写内容 | 模式+检索结果+指导 | 模式+检索结果                                       |
| 执行验证 | 模式+检索结果+指导 | 模式+检索结果+traceback中提到的有关操作件(规则匹配) |
| 未知     | 模式+检索结果+指导 | 模式+检索结果+                                      |

## 能力分析

以后再想
