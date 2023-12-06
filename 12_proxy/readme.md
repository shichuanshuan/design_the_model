# ����ģʽProxy
## ����
�������Ҫ�ڷ���һ������ʱ����һ���񡰴���һ���Ľ�ɫ���������ڷ��ʶ���֮ǰΪ����л����顢Ȩ���жϵȷ��ʿ��ƣ��ڷ��ʶ���֮��Ϊ����н�����桢��־��¼�Ƚ��������ô�Ϳ��Կ���ʹ�ô���ģʽ��

����һ��һЩweb��ܵ�routerģ�飬���ͻ��˷���һ���ӿ�ʱ��������ִ�ж�Ӧ�Ľӿ�֮ǰ��routerģ���ִ��һЩ��ǰ����������Ȩ���жϵȲ�������ִ��֮�󻹻��¼��־������ǵ��͵Ĵ���ģʽ��

## ���
����ģʽ��Ҫһ�������࣬�����ִ����ʵ��������ĳ�Ա���������ɴ�������������������ڡ�

�뿴���´��룺
```go
package proxy

import "fmt"

type Subject interface {
  Proxy() string
}

// ����

type Proxy struct {
  real RealSubject
}

func (p Proxy) Proxy() string {
  var res string

  // �ڵ�����ʵ����֮ǰ����黺�棬�ж�Ȩ�ޣ��ȵ�
  p.real.Pre()

  // ������ʵ����
  p.real.Real()

  // ����֮��Ĳ������绺�������Խ�����д����ȵ�
  p.real.After()

  return res
}

// ��ʵ����

type RealSubject struct{}

func (RealSubject) Real() {
  fmt.Print("real")
}

func (RealSubject) Pre() {
  fmt.Print("pre:")
}

func (RealSubject) After() {
  fmt.Print(":after")
}
```
���Ƕ����˴�����Proxy��ִ��Proxy֮���ڵ�����ʵ����Real֮ǰ�����ǻ��ȵ�����ǰ����Pre������ִ����ʵ����Real֮�󣬵����º����After��

���Դ��룺
```go
package proxy

func ExampleProxy() {
  var sub Subject
  sub = &Proxy{}
  sub.Proxy()

  // Output:
  // pre:real:after
}
```