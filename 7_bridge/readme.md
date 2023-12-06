# �Ž�ģʽBridge
## ����

����һ��ʼҵ����Ҫ���ַ�����Ϣ��������sms��email�����ǿ��Էֱ�ʵ��sms��email�����ӿڡ�

֮������ҵ��������ֲ������µ�������Ҫ�ṩ����ϵͳ���ͷ�ʽ��systemA��systemB������������ϵͳ���ͷ�ʽ��Ӧ��֧��sms��email������

��ʱ������Ҫ�ṩ4�ַ�����systemA to sms��systemA to email��systemB to sms��systemB to email��

����ٷֱ�����һ��������һ��ϵͳ���ͷ�ʽ������Ҫ�ṩ9�ַ������⽫���´���ĸ��ӳ̶�ָ��������

## ���
��ʵ֮ǰ���������ü̳е��뷨�������⣬�Ž�ģʽ��ϣ�����̳й�ϵת��Ϊ������ϵ��ʹ������������ڡ�

��ϸ˵һ�£�

�Ž�ģʽ��Ҫ�������ʵ�����ֿ���
�Ž�ģʽ��Ҫ�����������͡�ϵͳ���ͷ�ʽ��������������ֿ���
����ڡ�ϵͳ���ͷ�ʽ����������á��������ĳ���ӿڣ�ʹ���ǴӼ̳й�ϵת��Ϊ������ϵ��
��һ�仰�ܽ��Ž�ģʽ��������ǣ�����������ʵ�ֽ������ͬ���ļ̳й�ϵ��Ϊ������ϵ�� ��

�뿴���´��룺
```go
package bridge

import "fmt"

// ���ַ�����Ϣ�ķ���

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

// ���ַ���ϵͳ

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
���Կ��������ȶ�����sms��email����ʵ�֣��Լ��ӿ�SendMessage����������ʵ����systemA��systemB���������˳���ӿ�SendMessage��

���Դ��룺
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
�������Ҫ��ֻ�����һ�����ж��ع��ܵĸ����࣬����ʹ���Ž�ģʽ��