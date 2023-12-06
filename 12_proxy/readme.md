# 代理模式Proxy
## 问题
如果你需要在访问一个对象时，有一个像“代理”一样的角色，她可以在访问对象之前为你进行缓存检查、权限判断等访问控制，在访问对象之后为你进行结果缓存、日志记录等结果处理，那么就可以考虑使用代理模式。

回忆一下一些web框架的router模块，当客户端访问一个接口时，在最终执行对应的接口之前，router模块会执行一些事前操作，进行权限判断等操作，在执行之后还会记录日志，这就是典型的代理模式。

## 解决
代理模式需要一个代理类，其包含执行真实对象所需的成员变量，并由代理类管理整个生命周期。

请看以下代码：
```go
package proxy

import "fmt"

type Subject interface {
  Proxy() string
}

// 代理

type Proxy struct {
  real RealSubject
}

func (p Proxy) Proxy() string {
  var res string

  // 在调用真实对象之前，检查缓存，判断权限，等等
  p.real.Pre()

  // 调用真实对象
  p.real.Real()

  // 调用之后的操作，如缓存结果，对结果进行处理，等等
  p.real.After()

  return res
}

// 真实对象

type RealSubject struct{}

func (RealSubject) Real() {
  fmt.Print("real")
}

func (RealSubject) Pre() {
  fmt.Print("pre:")
}

func (RealSubject) After() {
  fmt.Print(":after")
}
```
我们定义了代理类Proxy，执行Proxy之后，在调用真实对象Real之前，我们会先调用事前对象Pre，并在执行真实对象Real之后，调用事后对象After。

测试代码：
```go
package proxy

func ExampleProxy() {
  var sub Subject
  sub = &Proxy{}
  sub.Proxy()

  // Output:
  // pre:real:after
}
```