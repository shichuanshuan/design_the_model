# 状态模式 State
## 问题
如果一个对象的实现方法会根据自身的状态而改变，就可以使用状态模式。

举个例子：假设有一个开门的方法，门的状态在一开始是“关闭”，你可以执行open方法和close方法，当你执行了open方法，门的状态就变成了“开启”，再执行open方法就不会执行开门的功能，而是返回“门已开启”，如果执行close方法，门的状态就变成了“关闭”，再执行close方法就不会执行关门的功能，而是返回“门已关闭”。这是一个简单的例子，我们将为每个状态提供不同的实现方法，将这些方法组织起来很麻烦，如果状态也越来越多呢？无疑，这将会使代码变得臃肿。

## 解决
如果我们需要为一个门对象提供3种状态下的open和close方法：

* “开启”状态下，open方法返回“门已开启”，close方法返回“关闭成功”。
* “关闭”状态下，open方法返回“开启成功”，close方法返回“门已关闭”。
* “损坏”状态下，open方法返回“门已损坏，无法开启”，close方法返回“门已损坏，无法关闭”。
请看以下代码：
```go
package state

import "fmt"

// 不同状态需要实现的接口

type state interface {
  open(*door)
  close(*door)
}

// 门对象

type door struct {
  opened  state
  closed  state
  damaged state

  currentState state // 当前状态
}

func (d *door) open() {
  d.currentState.open(d)
}

func (d *door) close() {
  d.currentState.close(d)
}

func (d *door) setState(s state) {
  d.currentState = s
}

// 开启状态

type opened struct{}

func (o *opened) open(d *door) {
  fmt.Println("门已开启")
}

func (o *opened) close(d *door) {
  fmt.Println("关闭成功")
}

// 关闭状态

type closed struct{}

func (c *closed) open(d *door) {
  fmt.Println("开启成功")
}

func (c *closed) close(d *door) {
  fmt.Println("门已关闭")
}

// 损坏状态

type damaged struct{}

func (a *damaged) open(d *door) {
  fmt.Println("门已损坏，无法开启")
}

func (a *damaged) close(d *door) {
  fmt.Println("门已损坏，无法关闭")
}
```
我们的门对象door实现了open和close方法，在方法中，只需要调用当前状态currentState的open和close方法即可。

测试代码：
```go
package state

func ExampleState() {
  door := &door{}

  // 开启状态
  opened := &opened{}
  door.setState(opened)
  door.open()
  door.close()

  // 关闭状态
  closed := &closed{}
  door.setState(closed)
  door.open()
  door.close()

  // 损坏状态
  damaged := &damaged{}
  door.setState(damaged)
  door.open()
  door.close()

  // Output:
  // 门已开启
  // 关闭成功
  // 开启成功
  // 门已关闭
  // 门已损坏，无法开启
  // 门已损坏，无法关闭
}
```