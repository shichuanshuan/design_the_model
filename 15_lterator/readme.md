# 迭代器模式Iterator
## 问题
迭代器模式用于遍历集合中的元素，无论集合的数据结构是怎样的。

## 解决
请看以下代码
```go
package iterator

// 集合接口

type collection interface {
  createIterator() iterator
}

// 具体的集合

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

// 迭代器

type iterator interface {
  hasNext() bool
  getNext() *part
}

// 具体的迭代器

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
测试代码：
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