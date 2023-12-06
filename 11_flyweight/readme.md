# 享元模式 Flyweight
## 问题
在一些情况下，程序没有足够的内存容量支持存储大量对象，或者大量的对象存储着重复的状态，此时就会造成内存资源的浪费。

享元模式提出了这样的解决方案：如果多个对象中相同的状态可以共用，就能在在有限的内存容量中载入更多对象。

## 解决
如上所说，享元模式希望抽取出能在多个对象间共享的重复状态。

我们可以使用map结构来实现这一设想，假设需要存储一些代表颜色的对象，使用享元模式可以这样做，请看以下代码：
```go
package flyweight

import "fmt"

// 享元工厂

type ColorFlyweightFactory struct {
  maps map[string]*ColorFlyweight
}

var colorFactory *ColorFlyweightFactory

func GetColorFlyweightFactory() *ColorFlyweightFactory {
  if colorFactory == nil {
    colorFactory = &ColorFlyweightFactory{
      maps: make(map[string]*ColorFlyweight),
    }
  }
  return colorFactory
}

func (f *ColorFlyweightFactory) Get(filename string) *ColorFlyweight {
  color := f.maps[filename]
  if color == nil {
    color = NewColorFlyweight(filename)
    f.maps[filename] = color
  }

  return color
}

type ColorFlyweight struct {
  data string
}

// 存储color对象

func NewColorFlyweight(filename string) *ColorFlyweight {
  // Load color file
  data := fmt.Sprintf("color data %s", filename)
  return &ColorFlyweight{
    data: data,
  }
}

type ColorViewer struct {
  *ColorFlyweight
}

func NewColorViewer(name string) *ColorViewer {
  color := GetColorFlyweightFactory().Get(name)
  return &ColorViewer{
    ColorFlyweight: color,
  }
}
```
我们定义了一个享元工厂，使用map存储相同对象（key）的状态（value）。这个享元工厂可以使我们更方便和安全的访问各种享元，保证其状态不被修改。

我们定义了NewColorViewer方法，它会调用享元工厂的Get方法存储对象，而在享元工厂的实现中可以看到，相同状态的对象只会占用一次。

测试代码：
```go
package flyweight

import "testing"

func TestFlyweight(t *testing.T) {
  viewer1 := NewColorViewer("blue")
  viewer2 := NewColorViewer("blue")

  if viewer1.ColorFlyweight != viewer2.ColorFlyweight {
    t.Fail()
  }
}
```
当程序需要存储大量对象且没有足够的内存容量时，可以考虑使用享元模式。