# IDE动作序列数据建模方法v0.1

[TOC]

# 方案1：1个模型预测具体操作

## 操作建模

```python
<操作向量> = torch.stack(
	<操作类型>, <当前操作件>, <父件>、<子件>、<调用了操作件的件>、<操作件调用的件>
)

<操作类型> = map[event_type]  # 数值映射

<当前操作件>, <父件>、<子件>、<调用了操作件的件>、<操作件调用的件> = 
torch.stack(artifact1, artifact2, artfact3...)  # 向量，长度为设定的最大长度，不足的值补0

artifact = 
map[artifact]  # 数值映射
```

## 端到端格式

**输入：一段操作序列**

```python
input,shape = (batch, time, in_features)
time = 最大回溯长度
in_features = <操作向量>
```

**输出：预测的一个操作**

```python
output.shape = (batch, out_features)
out_features = [<操作类型>，<操作件>]
```

## 向量化规则

每个event_type和artifact都映射成唯一的数值，映射可逆（要支持编解码），（可随时扩充）。event_type是预先定义的数值映射，有几种就映射到几个数。根据开发者的动作逐渐生成映射map，为每个相关的artifact生成唯一数值，用map存起来，每次程序员删除或添加一个artifact时，实时地更新这个map，具体为添加和删除kv

好处：如果收敛了，能一个模型cover整个功能；
缺点：每次增删操作件都要更新map，重新训练模型，因为模型输出跟具体的操作件挂钩了。



# 方案2：1+N，1个模型预测操作类型，分方案预测具体操作

## 操作建模

用具体的工件之间的相关性+环境反馈代替原本每个操作的具体工件，类似于差分，避免模型输出与频繁变动的操作件绑定。

这些内容要根据raw_data进一步预处理获得，但是在信息量足够的情况下只需要基于规则，并不复杂。

```python
<操作向量> = torch.stack(
	<操作类型>, <环境反馈信息>, <跳转方向>, <待补充>
)

<操作类型> = map[  # 数值映射
    fix_for_terminal,  # 终端报错，下一步要修复bug
    fix_for_static,  # 静态检查报错，下一步要修复bug
    note,  # 编辑注释
    tab,  # 代码补全
	search,   # 搜索
    replace,  # 替换
    edit_code,  # 编辑代码，除去修复问题、代码补全、搜索替换的编辑行为
    refactor,  # 重构符号名（IDE内置操作）
    rename_file,  # 重命名文件
    delete_file,  # 删除文件
    create_file,  # 新建文件
    move_file,  # 移动文件
    ask_agent,  # 人智交互，使用AI编程助手的相关功能
    read,  # 看文件看代码，连续滚动、选中等操作但是不编辑
    env,  # 配置环境
    action,  # 执行
    debug，  # 调试
]

<环境反馈信息> = binary_bool[  # 二进制编码后是唯一的数值
    terminal_env,  # 环境相关报错
    terminal_runtime,  # 程序运行时报错
    terminal_compile,  # 解释编译报错
    terminal_link,  # 终端的报错信息是否与前x步的操作件有关系
    static_error,  # 静态检查报错
]

<跳转方向> = binary_bool[  # 二进制编码后是唯一的数值
    on_current_artifact,  # 现在的操作件前x步内是不是操作过
    ref_son,  # 现在的操作件继承自前x步的操作件（多态）
    ref_father,  # 现在的操作件是前x步操作件的父类（多态）
    ref_rely,  # 现在的操作件调用了前x步的操作件
    ref_api,  # 现在的操作件被前x步的操作件调用
    ref_simlilar,  # 现在的操作件与前x步的操作件内容相似
]
```

## 端到端格式

**输入：一段操作序列**

```python
input,shape = (batch, time, in_features)
time = 最大回溯长度
in_features = <操作向量>
```

**输出：操作倾向性/状态**

```python
output.shape = (batch, out_featrues)
out_features = [<操作类型>, <跳转方向>]

<操作类型> = one-hot[同前]
<跳转方向> = one-hot[同前]
```

## 具体操作内容预测

对于每一类**操作类型**，有不同的对应的操作内容计算方法。要结合更加具体的**环境反馈**和**跳转方向**信息。

例如：

**fix_for_terminal**：预测出要进行终端报错的修复后，先获取终端报错信息，在Traceback中找到具体抛出错误的artifact，就是该操作对应的预测工件，fix_for_static同理，再结合跳转方向即可。
