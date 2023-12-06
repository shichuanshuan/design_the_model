# 桥接模式Bridge
## 问题

假设一开始业务需要两种发送信息的渠道，sms和email，我们可以分别实现sms和email两个接口。

之后随着业务迭代，又产生了新的需求，需要提供两种系统发送方式，systemA和systemB，并且这两种系统发送方式都应该支持sms和email渠道。

此时至少需要提供4种方法：systemA to sms，systemA to email，systemB to sms，systemB to email。

如果再分别增加一种渠道和一种系统发送方式，就需要提供9种方法。这将导致代码的复杂程度指数增长。

## 解决
其实之前我们是在用继承的想法来看问题，桥接模式则希望将继承关系转变为关联关系，使两个类独立存在。

详细说一下：

桥接模式需要将抽象和实现区分开；
桥接模式需要将“渠道”和“系统发送方式”这两种类别区分开；
最后在“系统发送方式”的类里调用“渠道”的抽象接口，使他们从继承关系转变为关联关系。
用一句话总结桥接模式的理念，就是：“将抽象与实现解耦，将不同类别的继承关系改为关联关系。 ”

请看以下代码：
```go
package bridge

import "fmt"

// 两种发送消息的方法

type SendMessage interface {
  send(text, to string)
}

type sms struct{}

func NewSms() SendMessage {
  return &sms{}
}

func (*sms) send(text, to string) {
  fmt.Println(fmt.Sprintf("send %s to %s sms", text, to))
}

type email struct{}

func NewEmail() SendMessage {
  return &email{}
}

func (*email) send(text, to string) {
  fmt.Println(fmt.Sprintf("send %s to %s email", text, to))
}

// 两种发送系统

type systemA struct {
  method SendMessage
}

func NewSystemA(method SendMessage) *systemA {
  return &systemA{
    method: method,
  }
}

func (m *systemA) SendMessage(text, to string) {
  m.method.send(fmt.Sprintf("[System A] %s", text), to)
}

type systemB struct {
  method SendMessage
}

func NewSystemB(method SendMessage) *systemB {
  return &systemB{
    method: method,
  }
}

func (m *systemB) SendMessage(text, to string) {
  m.method.send(fmt.Sprintf("[System B] %s", text), to)
}
```
可以看到我们先定义了sms和email二种实现，以及接口SendMessage。接着我们实现了systemA和systemB，并调用了抽象接口SendMessage。

测试代码：
```go
package bridge

func ExampleSystemA() {
  NewSystemA(NewSms()).SendMessage("hi", "baby")
  NewSystemA(NewEmail()).SendMessage("hi", "baby")
  // Output:
  // send [System A] hi to baby sms
  // send [System A] hi to baby email
}

func ExampleSystemB() {
  NewSystemB(NewSms()).SendMessage("hi", "baby")
  NewSystemB(NewEmail()).SendMessage("hi", "baby")
  // Output:
  // send [System B] hi to baby sms
  // send [System B] hi to baby email
}
```
如果你想要拆分或重组一个具有多重功能的复杂类，可以使用桥接模式。