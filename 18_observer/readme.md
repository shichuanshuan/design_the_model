# �۲���ģʽObserver
## ����
�������Ҫ��һ�������״̬���ı�ʱ��������������Ϊ�䡰�۲��ߡ�����֪ͨ���Ϳ���ʹ�ù۲���ģʽ��

���ǽ������״̬�ı�ͻ�֪ͨ����������Ķ����Ϊ�������ߡ�����ע������״̬�仯�Ķ������Ϊ�������ߡ���

## ���
�뿴���´��룺
```go
package observer

import "fmt"

// ������

type Subject struct {
  observers []Observer
  content   string
}

func NewSubject() *Subject {
  return &Subject{
    observers: make([]Observer, 0),
  }
}

// ��Ӷ�����

func (s *Subject) AddObserver(o Observer) {
  s.observers = append(s.observers, o)
}

// �ı䷢���ߵ�״̬

func (s *Subject) UpdateContext(content string) {
  s.content = content
  s.notify()
}

// ֪ͨ�����߽ӿ�

type Observer interface {
  Do(*Subject)
}

func (s *Subject) notify() {
  for _, o := range s.observers {
    o.Do(s)
  }
}

// ������

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
�ܼ򵥣�����ֻҪʵ��һ��֪ͨnotify�������ڷ����ߵ�״̬�ı�ʱִ�м��ɡ�

���Դ��룺
```go
package observer

func ExampleObserver() {
  subject := NewSubject()

  boy := NewReader("С��")
  girl := NewReader("С��")

  subject.AddObserver(boy)
  subject.AddObserver(girl)

  subject.UpdateContext("hi~")

  // Output:
  // С�� get hi~
  // С�� get hi~
}
```