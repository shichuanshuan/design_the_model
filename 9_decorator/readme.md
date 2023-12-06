# װ��ģʽDecorator
## ����
��ʱ��������Ҫ��һ����Ļ�������չ��һ���࣬���磬һ�������࣬�������������Ļ��������ӷ����������֥ʿ�����ࡣ��ʱ�Ϳ���ʹ��װ��ģʽ������˵��װ��ģʽ���ǽ������װ����һ�������У�����Ϊԭ������µ���Ϊ��

�����ϣ���������޸Ĵ���������ʹ�ö��󣬲���ϣ��Ϊ���������������Ϊ���Ϳ��Կ���ʹ��װ��ģʽ��

## ���
�����ĵ������������ӡ��뿴���´��룺
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
�������Ƕ�����pizza�ӿڣ�������base�࣬ʵ���˷���getPrice��Ȼ������װ��ģʽ�����ʵ����tomatoTopping��cheeseTopping�࣬���Ƕ���װ��pizza�ӿڵ�getPrice������

���Դ��룺
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