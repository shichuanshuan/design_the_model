# 策略模式Strategy
## 问题
假设需要实现一组出行的功能，出现的方案可以选择步行、骑行、开车，最简单的做法就是分别实现这3种方法供客户端调用。但这样做就使对象与其代码实现变得耦合了，客户端需要决定出行方式，然后决定调用步行出行、骑行出行、开车出行等方法，这不符合开闭原则。

而策略模式的区别在于，它会将这些出行方案抽取到一组被称为策略的类中，客户端还是调用同一个出行对象，不需要关注实现细节，只需要在参数中指定所需的策略即可。

## 解决
请看以下代码：
```go
package strategy

import "fmt"

type Travel struct {
  name     string
  strategy Strategy
}

func NewTravel(name string, strategy Strategy) *Travel {
  return &Travel{
    name:     name,
    strategy: strategy,
  }
}

func (p *Travel) traffic() {
  p.strategy.traffic(p)
}

type Strategy interface {
  traffic(*Travel)
}

type Walk struct{}

func (w *Walk) traffic(t *Travel) {
  fmt.Println(t.name + " walk")
}

type Ride struct{}

func (w *Ride) traffic(t *Travel) {
  fmt.Println(t.name + " ride")
}

type Drive struct{}

func (w *Drive) traffic(t *Travel) {
  fmt.Println(t.name + " drive")
}
```
我们定义了strategy一组策略接口，为其实现了Walk、Ride、Drive算法。客户端只需要执行traffic方法即可，无需关注实现细节。

测试代码：
```go
package strategy

func ExampleTravel() {
	walk := &Walk{}
	Travel1 := NewTravel("小明", walk)
	Travel1.traffic()

	ride := &Ride{}
	Travel2 := NewTravel("小美", ride)
	Travel2.traffic()

	drive := &Drive{}
	Travel3 := NewTravel("小刚", drive)
	Travel3.traffic()

	// Output:
	// 小明 walk
	// 小美 ride
	// 小刚 drive
}
```