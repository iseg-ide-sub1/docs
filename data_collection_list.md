# data_collection_list
## 1. event 类型

```ts
enum EventType {

OpenTextDocument = "Open text document",

CloseTextDocument = "Close text document",

AddTextDocument = "Add text document",

DeleteTextDocument = "Delete text document",

EditTextDocument = "Edit text document",

RedoTextDocument = "Redo text document",

UndoTextDocument = "Undo text document",

CreateFile = "Create a file",

DeleteFile = "Delete a file",

SaveFile = "Save a file"

}
```

## 2. 数据格式
每一个操作动作记录为一个json对象。示例如下：
```json
{

"id": 13,

"timeStamp": "2024-10-28 19:26:09.332",

"eventType": "Delete text document",

"artifact": {

"name": "setInnerData(int)",

"type": "Method",

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

"name": "InnerClass",

"type": "Class"

},

{

"name": "setInnerData(int)",

"type": "Method"

}

]

}

}
```
### 2.1. 属性

- id: number
- 时间戳timescript: string
- 事件类型eventType: EventType(自定义的枚举类型)
- 工件artifact: ArtiFact
	- 名字name: string
	- 类型type: ArtiFactType
	- 层次结构hierarchy?: ArtiFact[]
		- 注: 实际是一个工件列表，保存从最外层(文件级)到目前操作工件的层次结构