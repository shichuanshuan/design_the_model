# ������ģʽIterator
## ����
������ģʽ���ڱ��������е�Ԫ�أ����ۼ��ϵ����ݽṹ�������ġ�

## ���
�뿴���´���
```go
package iterator

// ���Ͻӿ�

type collection interface {
  createIterator() iterator
}

// ����ļ���

type part struct {
  title  string
  number int
}

type partCollection struct {
  part
  parts []*part
}

func (u *partCollection) createIterator() iterator {
  return &partIterator{
    parts: u.parts,
  }
}

// ������

type iterator interface {
  hasNext() bool
  getNext() *part
}

// ����ĵ�����

type partIterator struct {
  index int
  parts []*part
}

func (u *partIterator) hasNext() bool {
  if u.index < len(u.parts) {
    return true
  }
  return false

}
func (u *partIterator) getNext() *part {
  if u.hasNext() {
    part := u.parts[u.index]
    u.index++
    return part
  }
  return nil
}
```
���Դ��룺
```go
func ExampleIterator() {
  part1 := &part{
    title:  "part1",
    number: 10,
  }
  part2 := &part{
    title:  "part2",
    number: 20,
  }
  part3 := &part{
    title:  "part3",
    number: 30,
  }

  partCollection := &partCollection{
    parts: []*part{part1, part2, part3},
  }

  iterator := partCollection.createIterator()

  for iterator.hasNext() {
    part := iterator.getNext()
    fmt.Println(part)
  }

  // Output:
  // &{part1 10}
  // &{part2 20}
  // &{part3 30}
}
```