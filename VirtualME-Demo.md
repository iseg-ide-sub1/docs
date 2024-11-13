
# VirtualME-Demo
## 1. 使用方式

### 1.1. 基本介绍
基于VSCode开发了一个插件，可以监听用户在VSCode的操作，并为每种操作创建日志项，为后续监控和分析他们在开发环境中的行为模式、预测它们的意图提供数据基础。

### 1.2. 使用方式

1. 首次克隆仓库后，在控制台执行 `npm install` 安装项目依赖
2. 在编辑器中，打开 `src/extension.ts` 文件，然后按 `F5` 或者从命令面板运行`Debug: Start Debugging`。这将编译并运行扩展程序在一个新的扩展开发主机窗口中；
3. 按`Ctrl+Shift+P`（Windows）调出命令面板，运行命令`VirtualME Demo`就可以启动插件收集数据；
4. 运行的过程中，收集到的动作会实时log到控制台；
	![](./img/01.png)

4. 停止运行后，动作序列会被输出到一个json文件中。
	![](./img/02.png)


## 2. 数据结构

插件开发时自定义一个`LogItem`类用于记录每一个操作动作，并实现`toString`和`toJSON`方法，用于统一输出，以下是一个输出的json示例。

![](./img/03.png)

```json
{
	"id": 37,
	"timeStamp": "2024-10-29 16:49:31.287",
	"eventType": "Add text document",
	"artifact": {
		"name": "file:///Users/suyunhe/code/cpp/test10.cpp",
		"type": "File",
		"hierarchy": [
			{
				"name": "file:///Users/suyunhe/code/cpp/test10.cpp",
				"type": "File"
			},
			{
				"name": "OuterClass",
				"type": "Class"
			},
			{
				"name": "Inne1rClass",
				"type": "Class"
			}
		]
	},
	"detail": {
		"addContentLength": 29,
		"addContent": "// 方法中类示例11111\n// 方法中类示例11111"
	}
}
```

`LogItem`类具有以下属性:
```ts
id: number
timeStamp: string
eventType: EventType
artifact: ArtiFact
detail?: Map<string, any>
```

### 2.1. `id: number`
自增的log项id。每次从1开始自增。

### 2.2. `timeStamp: string`
时间戳，表示动作发生的具体时间。如：`2024-10-29 16:49:31.287`

### 2.3. `eventType: EventType`

事件类型，用于描述行为具体是什么，是一个自定义的枚举类。目前可收集到的动作如下：
```ts
enum EventType {
	/** 打开文件 */
	OpenTextDocument = "Open text document",
	/** 关闭文件 */
	CloseTextDocument = "Close text document",
	/** 切换文件 */
	ChangeTextDocument = "Change text document",
	/** 添加文件内容 */
	AddTextDocument = "Add text document",
	/** 删除文件内容 */
	DeleteTextDocument = "Delete text document",
	/** 修改文件内容 */
	EditTextDocument = "Edit text document",
	/** Redo文件内容 */
	RedoTextDocument = "Redo text document",
	/** Undo文件内容 */
	UndoTextDocument = "Undo text document",
	/** 新建文件 */
	CreateFile = "Create file",
	/** 删除文件 */
	DeleteFile = "Delete file",
	/** 保存文件 */
	SaveFile = "Save file",
	/** 选中文本 */
	SelectText = "Select text",
	/** 打开终端 */
	OpenTerminal = "Open terminal",
	/** 关闭终端 */
	CloseTerminal = "Close terminal",
	/** 切换终端 */
	ChangeActiveTerminal = "Change active terminal",
}
```

### 2.4. `artifact: ArtiFact`
工件，用于描述本次行为操作了哪个工件，是一个自定义类。
`Artifact`类具有以下属性：
```ts
name: string
type: ArtiFactType
hierarchy?: ArtiFact[]
public context?: Context
```

#### 2.4.1. `name: string`
工件的名字，string类型。

#### 2.4.2. `type: ArtiFactType`
工件的类型，是一个自定义的枚举类。目前工件类型主要借鉴了vscode api源码中的定义，如下：
```ts
export enum ArtiFactType {
	File = "File",
	Module = "Module",
	Namespace = "Namespace",
	Package = "Package",
	Class = "Class",
	Method = "Method",
	Property = "Property",
	Field = "Field",
	Constructor = "Constructor",
	Enum = "Enum",
	Interface = "Interface",
	Function = "Function",
	Variable = "Variable",
	Constant = "Constant",
	String = "String",
	Number = "Number",
	Boolean = "Boolean",
	Array = "Array",
	Object = "Object",
	Key = "Key",
	Null = "Null",
	EnumMember = "EnumMember",
	Struct = "Struct",
	Event = "Event",
	Operator = "Operator",
	TypeParameter = "TypeParameter",
	Terminal = "Terminal", // ?
	Unknown = "Unknown"
}
```


#### 2.4.3. hierarchy?: ArtiFact[]
工件所在的层级。实际是一个工件列表，用于保存从最外层(文件级)到目前操作工件的层次结构。

例如，对于`test.cpp`中的以下代码：
```cpp
class OuterClass {
	private:
		int outerData;
		int test;
	public:
		// 嵌套类
		class InnerClass {
			private:
				int innerData;
			public:
				InnerClass() : innerData(0) {}
				InnerClass(int value) : innerData(value) {}
				void setInnerData(int value) { innerData = value; }
				int getInnerData() const { return innerData; }
		}
	};
};
```

`InnerClass`的层次可以描述为`test.cpp` -> `OuterClass` -> `InnerClass`

### 2.5. `detail?: Map<string, any>`

用于按需存储其他信息。例如对于**添加文件内容**可以保存添加内容，**切换终端**可以保存终端对应的进程号等等。

## 3. 事件类型

### 3.1 `OpenTextDocument` 打开文件

`vscode.workspace.onDidOpenTextDocument`

打开文本文件时触发

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

### 3.2 `CloseTextDocument` 关闭文件

`vscode.workspace.onDidCloseTextDocument`

关闭文本文件时触发

```json
  {
    "id": 1,
    "timeStamp": "2024-11-11 15:28:27.566",
    "eventType": "Close text document",
    "artifact": {
      "name": "file:///c%3A/Users/hiron/Desktop/Code/test.c",
      "type": "File"
    },
    "detail": {}
  }
```

### 3.3 `ChangeTextDocument` 切换文件

`vscode.window.onDidChangeActiveTextEditor`

切换文本文件时触发，若当前关闭所有编辑视图，`editor` 值为 `undefined`

切换编辑视图，会触发两次此事件，第一次 `editor` 值为 `undefined`

插件不会记录 `editor` 值为 `undefined` 的情况

```json
  {
    "id": 3,
    "timeStamp": "2024-11-11 15:28:28.243",
    "eventType": "Change text document",
    "artifact": {
      "name": "file:///c%3A/Users/hiron/Desktop/Code/test2.c",
      "type": "File"
    },
    "detail": {}
  }
```

### 3.4 `AddTextDocument` 添加文件内容

`vscode.workspace.onDidChangeTextDocument`



### 3.5 `DeleteTextDocument` 删除文件内容

`vscode.workspace.onDidChangeTextDocument`



### 3.6 `EditTextDocument` 修改文件内容

`vscode.workspace.onDidChangeTextDocument`



### 3.7 `RedoTextDocument` Redo文件内容

`vscode.workspace.onDidChangeTextDocument`



### 3.8 `UndoTextDocument` Undo文件内容

`vscode.workspace.onDidChangeTextDocument`

 

### 3.9 `CreateFile` 新建文件

```typescript
const filesWatcher = vscode.workspace.createFileSystemWatcher('**/*')
filesWatcher.onDidCreate
```



```json
  {
    "id": 6,
    "timeStamp": "2024-11-11 17:00:56.314",
    "eventType": "Create file",
    "artifact": {
      "name": "file:///c%3A/Users/hiron/Desktop/Code/new-f.c",
      "type": "File"
    }
  }
```

### 3.10 `DeleteFile` 删除文件

```typescript
const filesWatcher = vscode.workspace.createFileSystemWatcher('**/*')
filesWatcher.onDidDelete
```



```json
  {
    "id": 11,
    "timeStamp": "2024-11-11 17:01:40.644",
    "eventType": "Delete file",
    "artifact": {
      "name": "file:///c%3A/Users/hiron/Desktop/Code/del.c",
      "type": "File"
    },
    "detail": {}
  }
```

### 3.11 `SaveFile` 保存文件

```typescript
const filesWatcher = vscode.workspace.createFileSystemWatcher('**/*')
filesWatcher.onDidChange
```





```json
  {
    "id": 10,
    "timeStamp": "2024-11-11 17:01:09.189",
    "eventType": "Save file",
    "artifact": {
      "name": "file:///c%3A/Users/hiron/Desktop/Code/new-f.c",
      "type": "File"
    },
    "detail": {}
  }
```

### 3.12 `SelectText` 选中文本

`vscode.window.onDidChangeTextEditorSelection`

鼠标、键盘、命令都可能触发该事件，仅仅移动光标，也会触发此事件

插件进行了处理，只有选择的文本内容不为空时才记录

**问题：**在使用鼠标或键盘进行选择时，选择区域每扩大一次就会记录一次选中，这使得记录内容暴增，考虑优化记录逻辑

```json
  {
    "id": 30,
    "timeStamp": "2024-11-11 16:54:41.866",
    "eventType": "Select text",
    "artifact": {
      "name": "main()",
      "type": "Function",
      "hierarchy": [
        {
          "name": "file:///c%3A/Users/hiron/Desktop/Code/test2.c",
          "type": "File",
          "context": {
            "position": {
              "line": 4,
              "character": 5
            },
            "scope": {
              "file": {
                "name": "test2.c",
                "path": "c:\\Users\\hiron\\Desktop\\Code\\test2.c"
              },
              "method": {
                "name": "main()"
              }
            }
          }
        },
        {
          "name": "main()",
          "type": "Function",
          "context": {
            "position": {
              "line": 4,
              "character": 5
            },
            "scope": {
              "file": {
                "name": "test2.c",
                "path": "c:\\Users\\hiron\\Desktop\\Code\\test2.c"
              },
              "method": {
                "name": "main()"
              }
            }
          }
        }
      ],
      "context": {
        "selection": {
          "text": "scanf(\"%d%d\",",
          "range": {
            "start": {
              "line": 4,
              "character": 5
            },
            "end": {
              "line": 4,
              "character": 18
            }
          }
        }
      }
    },
    "detail": {}
  }
```

### 3.13 `OpenTerminal` 打开终端

`vscode.window.onDidOpenTerminal`

打开终端时触发

**问题：**当前无法记录打开终端的名称，待修复（原因可能是在获取名称时终端还没有初始化完成）

```json
  {
    "id": 9,
    "timeStamp": "2024-11-11 16:34:38.093",
    "eventType": "Open terminal",
    "artifact": {
      "name": "",
      "type": "Terminal"
    },
    "detail": {
      "processId": 10424
    }
  },
```

### 3.14 `CloseTerminal` 关闭终端

`vscode.window.onDidCloseTerminal`

```json
  {
    "id": 21,
    "timeStamp": "2024-11-11 16:35:09.610",
    "eventType": "Close terminal",
    "artifact": {
      "name": "bash",
      "type": "Terminal"
    },
    "detail": {
      "processId": 18424
    }
  }
```

### 3.15 `ChangeActiveTerminal` 切换终端

`vscode.window.onDidChangeActiveTerminal`

```json
  {
    "id": 11,
    "timeStamp": "2024-11-11 16:34:51.144",
    "eventType": "Change active terminal",
    "artifact": {
      "name": "powershell",
      "type": "Terminal"
    },
    "detail": {
      "processId": 10424
    }
  }
```

