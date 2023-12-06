# ����¼ģʽMemento
## ����
���õ����ֱ༭����֧�ֱ���ͻָ�һ�����ֵĲ��������������Ҫ�ڳ�����ʵ�ֱ���ͻָ��Ĺ��ܸ���ô���أ�

������Ҫ�ṩ����ͻָ��Ĺ��ܣ������湦�ܱ�����ʱ���ͻ����ɵ�ǰ����Ŀ��գ��ڻָ����ܱ�����ʱ���ͻ���֮ǰ����Ŀ��ո��ǵ�ǰ�Ŀ��ա������ʹ�ñ���¼ģʽ������

## ���
�뿴���´��룺
```go
package memento

import "fmt"

type Memento interface{}

type Text struct {
  content string
}

type textMemento struct {
  content string
}

func (t *Text) Write(content string) {
  t.content = content
}

func (t *Text) Save() Memento {
  return &textMemento{
    content: t.content,
  }
}

func (t *Text) Load(m Memento) {
  tm := m.(*textMemento)
  t.content = tm.content
}

func (t *Text) Show() {
  fmt.Println("content:", t.content)
}
```
���Ƕ�����textMemento�ṹ�����ڱ��浱ǰ���գ�����Load�����н����ո��ǵ���ǰ���ݡ�

���Դ��룺
```go
package memento

func ExampleText() {
  text := &Text{
    content: "how are you",
  }

  text.Show()
  progress := text.Save()

  text.Write("fine think you and you")
  text.Show()

  text.Load(progress)
  text.Show()

  // Output:
  // content: how are you
  // content: fine think you and you
  // content: how are you
}
```