# 责任链模式Chain of Responsibility
## 问题
假设我们要让程序按照指定的步骤执行，并且这个步骤的顺序不是固定的，而是可以根据不同需求改变的，每个步骤都会对请求进行一些处理，并将结果传递给下一个步骤的处理者，就像一条流水线一样，我们该如何实现？

当遇到这种必须按顺序执行多个处理者，并且处理者的顺序可以改变的需求，我们可以考虑使用责任链模式。

## 解决
责任链模式使用了类似链表的结构。请看以下代码：
```go
package chain

import "fmt"

type department interface {
  execute(*Do)
  setNext(department)
}

type aPart struct {
  next department
}

func (r *aPart) execute(p *Do) {
  if p.aPartDone {
    fmt.Println("aPart done")
    r.next.execute(p)
    return
  }
  fmt.Println("aPart")
  p.aPartDone = true
  r.next.execute(p)
}

func (r *aPart) setNext(next department) {
  r.next = next
}

type bPart struct {
  next department
}

func (d *bPart) execute(p *Do) {
  if p.bPartDone {
    fmt.Println("bPart done")
    d.next.execute(p)
    return
  }
  fmt.Println("bPart")
  p.bPartDone = true
  d.next.execute(p)
}

func (d *bPart) setNext(next department) {
  d.next = next
}

type endPart struct {
  next department
}

func (c *endPart) execute(p *Do) {
  if p.endPartDone {
    fmt.Println("endPart Done")
  }
  fmt.Println("endPart")
}

func (c *endPart) setNext(next department) {
  c.next = next
}

type Do struct {
  aPartDone   bool
  bPartDone   bool
  endPartDone bool
}
```
我们实现了方法execute和setNext，并定义了aPart、bPart、endPart这3个处理者，每个处理者都可以通过execute方法执行其对应的业务代码，并可以通过setNext方法决定下一个处理者是谁。除了endPart是最终的处理者之外，在它之前的处理者aPart、bPart的顺序都可以任意调整。

请看以下测试代码：
```go
func ExampleChain() {
  startPart := &endPart{}

  aPart := &aPart{}
  aPart.setNext(startPart)

  bPart := &bPart{}
  bPart.setNext(aPart)

  do := &Do{}
  bPart.execute(do)

  // Output:
  // bPart
  // aPart
  // endPart
}
```
我们也可以调整处理者的执行顺序：
```go
func ExampleChain2() {
  startPart := &endPart{}

  bPart := &bPart{}
  bPart.setNext(startPart)

  aPart := &aPart{}
  aPart.setNext(bPart)

  do := &Do{}
  aPart.execute(do)

  // Output:
  // aPart
  // bPart
  // endPart
}
```