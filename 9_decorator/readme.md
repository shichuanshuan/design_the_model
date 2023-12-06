# 装饰模式Decorator
## 问题
有时候我们需要在一个类的基础上扩展另一个类，例如，一个披萨类，你可以在披萨类的基础上增加番茄披萨类和芝士披萨类。此时就可以使用装饰模式，简单来说，装饰模式就是将对象封装到另一个对象中，用以为原对象绑定新的行为。

如果你希望在无需修改代码的情况下使用对象，并且希望为对象新增额外的行为，就可以考虑使用装饰模式。

## 解决
用上文的披萨类做例子。请看以下代码：
```go
package decorator

type pizza interface {
  getPrice() int
}

type base struct {}

func (p *base) getPrice() int {
  return 15
}

type tomatoTopping struct {
  pizza pizza
}

func (c *tomatoTopping) getPrice() int {
  pizzaPrice := c.pizza.getPrice()
  return pizzaPrice + 10
}

type cheeseTopping struct {
  pizza pizza
}

func (c *cheeseTopping) getPrice() int {
  pizzaPrice := c.pizza.getPrice()
  return pizzaPrice + 20
}
```
首先我们定义了pizza接口，创建了base类，实现了方法getPrice。然后再用装饰模式的理念，实现了tomatoTopping和cheeseTopping类，他们都封装了pizza接口的getPrice方法。

测试代码：
```go
package decorator

import "fmt"

func ExampleDecorator() {

  pizza := &base{}

  //Add cheese topping
  pizzaWithCheese := &cheeseTopping{
    pizza: pizza,
  }

  //Add tomato topping
  pizzaWithCheeseAndTomato := &tomatoTopping{
    pizza: pizzaWithCheese,
  }

  fmt.Printf("price is %d\n", pizzaWithCheeseAndTomato.getPrice())

  // Output:
  // price is 45
}
```