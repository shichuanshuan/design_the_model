# 原型模式 Prototype
## 问题
如果你希望生成一个对象，其与另一个对象完全相同，该如何实现呢？

如果遍历对象的所有成员，将其依次复制到新对象中，会稍显麻烦，而且有些对象可能会有私有成员变量遗漏。

原型模式将这个克隆的过程委派给了被克隆的实际对象，被克隆的对象就叫做“原型”。

## 解决
如果需要克隆一个新的对象，这个对象完全独立于它的原型，那么就可以使用原型模式。

原型模式的实现非常简单，请看以下代码：
```go
package prototype

import "testing"

var manager *PrototypeManager

type Type1 struct {
  name string
}

func (t *Type1) Clone() *Type1 {
  tc := *t
  return &tc
}

func TestClone(t *testing.T) {
  t1 := &Type1{
    name: "type1",
  }

  t2 := t1.Clone()

  if t1 == t2 {
    t.Fatal("error! get clone not working")
  }
}
```
我们依靠一个Clone方法实现了原型Type1的克隆。

原型模式的用处就在于我们可以克隆对象，而无需与原型对象的依赖相耦合。