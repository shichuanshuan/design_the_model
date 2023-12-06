# 对象树模式Object Tree
## 问题
在项目中，如果我们需要用到树状结构，就可以使用对象树模式。换言之，如果项目的核心模型不能以树状结构表示，则没必要使用对象树模式。

对象树模式的用处就在于可以利用多态和递归机制更方便地使用复杂树结构

## 解决
请看以下代码:
```go
package objecttree

import "fmt"

type Component interface {
  Parent() Component
  SetParent(Component)
  Name() string
  SetName(string)
  AddChild(Component)
  Search(string)
}

const (
  LeafNode = iota
  CompositeNode
)

func NewComponent(kind int, name string) Component {
  var c Component
  switch kind {
  case LeafNode:
    c = NewLeaf()
  case CompositeNode:
    c = NewComposite()
  }

  c.SetName(name)
  return c
}

type component struct {
  parent Component
  name   string
}

func (c *component) Parent() Component {
  return c.parent
}

func (c *component) SetParent(parent Component) {
  c.parent = parent
}

func (c *component) Name() string {
  return c.name
}

func (c *component) SetName(name string) {
  c.name = name
}

func (c *component) AddChild(Component) {}

type Leaf struct {
  component
}

func NewLeaf() *Leaf {
  return &Leaf{}
}

func (c *Leaf) Search(pre string) {
  fmt.Printf("leaf %s-%s\n", pre, c.Name())
}

type Composite struct {
  component
  childs []Component
}

func NewComposite() *Composite {
  return &Composite{
    childs: make([]Component, 0),
  }
}

func (c *Composite) AddChild(child Component) {
  child.SetParent(c)
  c.childs = append(c.childs, child)
}

func (c *Composite) Search(pre string) {
  fmt.Printf("%s+%s\n", pre, c.Name())
  pre += " "
  for _, comp := range c.childs {
    comp.Search(pre)
  }
}
```
在Search方法中使用递归打印出了整棵树结构。

测试代码：
```go
package objecttree

func ExampleComposite() {
  root := NewComponent(CompositeNode, "root")
  c1 := NewComponent(CompositeNode, "c1")
  c2 := NewComponent(CompositeNode, "c2")
  c3 := NewComponent(CompositeNode, "c3")

  l1 := NewComponent(LeafNode, "l1")
  l2 := NewComponent(LeafNode, "l2")
  l3 := NewComponent(LeafNode, "l3")

  root.AddChild(c1)
  root.AddChild(c2)
  c1.AddChild(c3)
  c1.AddChild(l1)
  c2.AddChild(l2)
  c2.AddChild(l3)

  root.Search("")
  // Output:
  // +root
  //  +c1
  //   +c3
  //leaf   -l1
  //  +c2
  //leaf   -l2
  //leaf   -l3
}
```