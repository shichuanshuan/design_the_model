# 观察者模式Observer
## 问题
如果你需要在一个对象的状态被改变时，其他对象能作为其“观察者”而被通知，就可以使用观察者模式。

我们将自身的状态改变就会通知给其他对象的对象称为“发布者”，关注发布者状态变化的对象则称为“订阅者”。

## 解决
请看以下代码：
```go
package observer

import "fmt"

// 发布者

type Subject struct {
  observers []Observer
  content   string
}

func NewSubject() *Subject {
  return &Subject{
    observers: make([]Observer, 0),
  }
}

// 添加订阅者

func (s *Subject) AddObserver(o Observer) {
  s.observers = append(s.observers, o)
}

// 改变发布者的状态

func (s *Subject) UpdateContext(content string) {
  s.content = content
  s.notify()
}

// 通知订阅者接口

type Observer interface {
  Do(*Subject)
}

func (s *Subject) notify() {
  for _, o := range s.observers {
    o.Do(s)
  }
}

// 订阅者

type Reader struct {
  name string
}

func NewReader(name string) *Reader {
  return &Reader{
    name: name,
  }
}

func (r *Reader) Do(s *Subject) {
  fmt.Println(r.name + " get " + s.content)
}
```
很简单，我们只要实现一个通知notify方法，在发布者的状态改变时执行即可。

测试代码：
```go
package observer

func ExampleObserver() {
	subject := NewSubject()

	boy := NewReader("小明")
	girl := NewReader("小美")

	subject.AddObserver(boy)
	subject.AddObserver(girl)

	subject.UpdateContext("hi~")

	// Output:
	// 小明 get hi~
	// 小美 get hi~
}
```