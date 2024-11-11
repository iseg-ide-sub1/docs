# 插件架构

## 事件总览

### 文件级事件

| 编号 | 名称           | 符号                 | 开发人员 | 是否实现 |
| ---- | -------------- | -------------------- | -------- | -------- |
| 1-1  | 打开文本文件   | `OpenTextDocument`   |          |          |
| 1-2  | 关闭文本文件   | `CloseTextDocument`  |          |          |
| 1-3  | 切换文本编辑器 | `ChangeTextDocument` |          |          |
| 1-4  | 新建文件       | `CreateFile`         |          |          |
| 1-5  | 删除文件       | `DeleteFile`         |          |          |
| 1-6  | 保存文件       | `SaveFile`           |          |          |

### 文本内容相关事件

| 编号 | 名称         | 符号                 | 开发人员 | 是否实现 |
| ---- | ------------ | -------------------- | -------- | -------- |
| 2-1  | 添加文件内容 | `AddTextDocument`    |          |          |
| 2-2  | 删除文件内容 | `DeleteTextDocument` |          |          |
| 2-3  | 修改文件内容 | `EditTextDocument`   |          |          |

### 其他事件

| 编号 | 名称 | 符号 | 开发人员 | 是否实现 |
| ---- | ---- | ---- | -------- | -------- |
|      |      |      |          |          |

## 事件属性

### 通用属性

以下属性为通用属性，每个事件类型都会包含

- `id: number`  在本次记录中的序号
- `timeStamp: string`  记录本事件的时间戳
- `eventType: EventType`  事件类型
- `artifact: Artifact`  操作工件
  - `name: string` 工件名称
  - `type: ArtifactType` 工件类型 
  - `hierarchy?: Artifact[]` 该工件的层级
- `detail?: Map<string, any>` 该事件类型的附加信息

### 1-1 `OpenTextDocument`

**实现API：**`vscode.workspace.onDidOpenTextDocument`

**事件描述：**打开文本文件时触发

**附加属性：**无

**示例数据：**

```json
  {
    "id": 1,
    "timeStamp": "2024-11-11 15:25:19.823",
    "eventType": "Open text document",
    "artifact": {
      "name": "file:///c%3A/Users/hiron/Desktop/Code/test.c",
      "type": "File"
    },
    "detail": {}
  }
```

## 插件代码组织结构

### `extension.ts`

插件入口

### `log-item.ts`

事件数据结构

### `utils-common.ts`

保存通用函数

### `utils-process.ts`

保存数据预处理函数

## 数据保存约定

