# 中介者模式Mediator
## 问题
中介者模式试图解决网状关系的复杂关联，降低对象间的耦合度。

举个例子，假设一个十字路口上的车都是对象，它们会执行不同的操作，前往不同的目的地，那么在十字路口指挥的交警就是“中介者”。

各个对象通过执行中介者接口，再由中介者维护对象之间的联系。这能使对象变得更独立，比较适合用在一些对象是网状关系的案例上。

## 解决
假设有p1，p2，p3这3个发送者，p1 发送的消息p2能收到，p2 发送的消息p1能收到，p3 发送的消息则p1和p2能收到，如何实现呢？像这种情况就很适合用中介者模式实现。

请看以下代码：
```go
package mediator

import (
  "fmt"
)

type p1 struct{}

func (p *p1) getMessage(data string) {
  fmt.Println("p1 get message: " + data)
}

type p2 struct{}

func (p *p2) getMessage(data string) {
  fmt.Println("p2 get message: " + data)
}

type p3 struct{}

func (p *p3) getMessage(data string) {
  fmt.Println("p3 get message: " + data)
}

type Message struct {
  p1 *p1
  p2 *p2
  p3 *p3
}

func (m *Message) sendMessage(i interface{}, data string) {
  switch i.(type) {
  case *p1:
    m.p2.getMessage(data)
  case *p2:
    m.p1.getMessage(data)
  case *p3:
    m.p1.getMessage(data)
    m.p2.getMessage(data)
  }
}
```
我们定义了p1，p2，p3这3个对象，然后实现了中介者sendMessage。

测试代码：
```go
package mediator

func ExampleMediator() {
	message := &Message{}

	p1 := &p1{}
	p2 := &p2{}
	p3 := &p3{}

	message.sendMessage(p1, "hi! my name is p1")
	message.sendMessage(p2, "hi! my name is p2")
	message.sendMessage(p3, "hi! my name is p3")

	// Output:
	// p2 get message: hi! my name is p1
	// p1 get message: hi! my name is p2
	// p1 get message: hi! my name is p3
	// p2 get message: hi! my name is p3
}
```