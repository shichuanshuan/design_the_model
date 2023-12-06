# �н���ģʽMediator
## ����
�н���ģʽ��ͼ�����״��ϵ�ĸ��ӹ��������Ͷ�������϶ȡ�

�ٸ����ӣ�����һ��ʮ��·���ϵĳ����Ƕ������ǻ�ִ�в�ͬ�Ĳ�����ǰ����ͬ��Ŀ�ĵأ���ô��ʮ��·��ָ�ӵĽ������ǡ��н��ߡ���

��������ͨ��ִ���н��߽ӿڣ������н���ά������֮�����ϵ������ʹ�����ø��������Ƚ��ʺ�����һЩ��������״��ϵ�İ����ϡ�

## ���
������p1��p2��p3��3�������ߣ�p1 ���͵���Ϣp2���յ���p2 ���͵���Ϣp1���յ���p3 ���͵���Ϣ��p1��p2���յ������ʵ���أ�����������ͺ��ʺ����н���ģʽʵ�֡�

�뿴���´��룺
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
���Ƕ�����p1��p2��p3��3������Ȼ��ʵ�����н���sendMessage��

���Դ��룺
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