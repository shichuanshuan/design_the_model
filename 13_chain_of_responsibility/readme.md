# ������ģʽChain of Responsibility
## ����
��������Ҫ�ó�����ָ���Ĳ���ִ�У�������������˳���ǹ̶��ģ����ǿ��Ը��ݲ�ͬ����ı�ģ�ÿ�����趼����������һЩ��������������ݸ���һ������Ĵ����ߣ�����һ����ˮ��һ�������Ǹ����ʵ�֣�

���������ֱ��밴˳��ִ�ж�������ߣ����Ҵ����ߵ�˳����Ըı���������ǿ��Կ���ʹ��������ģʽ��

## ���
������ģʽʹ������������Ľṹ���뿴���´��룺
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
����ʵ���˷���execute��setNext����������aPart��bPart��endPart��3�������ߣ�ÿ�������߶�����ͨ��execute����ִ�����Ӧ��ҵ����룬������ͨ��setNext����������һ����������˭������endPart�����յĴ�����֮�⣬����֮ǰ�Ĵ�����aPart��bPart��˳�򶼿������������

�뿴���²��Դ��룺
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
����Ҳ���Ե��������ߵ�ִ��˳��
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