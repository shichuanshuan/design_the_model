# 备忘录模式Memento
## 问题
常用的文字编辑器都支持保存和恢复一段文字的操作，如果我们想要在程序中实现保存和恢复的功能该怎么做呢？

我们需要提供保存和恢复的功能，当保存功能被调用时，就会生成当前对象的快照，在恢复功能被调用时，就会用之前保存的快照覆盖当前的快照。这可以使用备忘录模式来做。

## 解决
请看以下代码：
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
我们定义了textMemento结构体用于保存当前快照，并在Load方法中将快照覆盖到当前内容。

测试代码：
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