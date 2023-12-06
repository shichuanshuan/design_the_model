# ����ģʽStrategy
## ����
������Ҫʵ��һ����еĹ��ܣ����ֵķ�������ѡ���С����С���������򵥵��������Ƿֱ�ʵ����3�ַ������ͻ��˵��á�����������ʹ�����������ʵ�ֱ������ˣ��ͻ�����Ҫ�������з�ʽ��Ȼ��������ò��г��С����г��С��������еȷ������ⲻ���Ͽ���ԭ��

������ģʽ���������ڣ����Ὣ��Щ���з�����ȡ��һ�鱻��Ϊ���Ե����У��ͻ��˻��ǵ���ͬһ�����ж��󣬲���Ҫ��עʵ��ϸ�ڣ�ֻ��Ҫ�ڲ�����ָ������Ĳ��Լ��ɡ�

## ���
�뿴���´��룺
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
���Ƕ�����strategyһ����Խӿڣ�Ϊ��ʵ����Walk��Ride��Drive�㷨���ͻ���ֻ��Ҫִ��traffic�������ɣ������עʵ��ϸ�ڡ�

���Դ��룺
```go
package strategy

func ExampleTravel() {
  walk := &Walk{}
  Travel1 := NewTravel("С��", walk)
  Travel1.traffic()

  ride := &Ride{}
  Travel2 := NewTravel("С��", ride)
  Travel2.traffic()

  drive := &Drive{}
  Travel3 := NewTravel("С��", drive)
  Travel3.traffic()

  // Output:
  // С�� walk
  // С�� ride
  // С�� drive
}
```