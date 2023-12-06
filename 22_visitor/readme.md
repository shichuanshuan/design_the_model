# 访问者模式Visitor
## 问题
访问者模式试图解决这样一个问题：在不改变类的对象结构的前提下增加新的操作。

## 解决
请看以下代码：
```go
package visitor

import "fmt"

type Shape interface {
  accept(visitor)
}

type square struct{}

func (s *square) accept(v visitor) {
  v.visitForSquare(s)
}

type circle struct{}

func (c *circle) accept(v visitor) {
  v.visitForCircle(c)
}

type visitor interface {
  visitForSquare(*square)
  visitForCircle(*circle)
}

type sideCalculator struct{}

func (a *sideCalculator) visitForSquare(s *square) {
  fmt.Println("square side")
}

func (a *sideCalculator) visitForCircle(s *circle) {
  fmt.Println("circle side")
}

type radiusCalculator struct{}

func (a *radiusCalculator) visitForSquare(s *square) {
  fmt.Println("square radius")
}

func (a *radiusCalculator) visitForCircle(c *circle) {
  fmt.Println("circle radius")
}
```
测试代码：
```go
package visitor

func ExampleShape() {
	square := &square{}
	circle := &circle{}

	side := &sideCalculator{}

	square.accept(side)
	circle.accept(side)

	radius := &radiusCalculator{}
	square.accept(radius)
	circle.accept(radius)

	// Output:
	// square side
	// circle side
	// square radius
	// circle radius
}
```