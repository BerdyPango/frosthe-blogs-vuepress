
## Scenes

## Nodes

## Scripts

Scripts 是附加在 Node 上的脚本代码，用以扩展 Node 的功能。每个 Script 代表着直接或间接继承了 Godot 引擎内置的类型。

### 虚函数笔记

基于约定，带有下划线为前缀的函数名为 Godot 引擎的内置虚函数。这意味着我们可以重写并扩展这些函数的行为。

- `func _init()`: `Node._init()` 虚函数，被引擎识别为类型构造函数。引擎会在构造每个对象时调用该函数。
- `func _ready()`: `Node._ready()` 虚函数，当 Node 及其子节点全部准备好时被引擎调用
- `func _process(delta)`: `Node._process(delta)` 虚函数，可被继承类型重写。引擎会在每一帧渲染时调用该函数，并传入一个 `delta` 参数，代表距离上一帧渲染的差值。
- `func _unhandled_inpuit()`: 当玩家输入时，引擎将调用该函数。

### 公共函数笔记

- `get_node(path)`: 以当前节点根据 Node 名称获取 Node 实例的引用，通过相对路径查找，例如 `get_node("Timer")`，"Timer" 字面值为该 Node 的名称，如果其修改名称，则需要相应修改这个字符串值。

### 语法关键字

- `extends`: 继承自某类型
- `var`: 声明变量
- `signal`: 声明自定义 Signal，一般命名约定为动词过去时，如 `signal health_depleted`。
  - Signal 声明时可包含参数，如 `signal health_depleted(old_value, new_value)`。
  - `.emit()` 函数: 每个声明的 Signal 都带有一个 `emit()` 函数，调用时发布该 Signal。若 signal 声明包含参数，则 emit 调用时也应对应传入这些参数：`emit(old_value, new_value)`
- `@export`: 使用该关键字导出的变量允许该属性由 Godot 引擎的 Inspector 在 GUI 界面上调整。例如 `@export var speed = 400`


## Signals

Signals 是游戏对象 (Scene 或 Node) 之间传递消息的机制，其基于观察者或 (发布/订阅) 模式实现。例如当 Node A 的某个按钮被按下后可以发出 Signal，Node B 或 C 或更多的 Node 可以侦听该 Signal，以实现通信。

- Signal 侦听处理方法的命名约定: `_on_node_name_signal_name`

## Resources