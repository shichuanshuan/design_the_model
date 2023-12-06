# 外观模式Facade
## 问题
如果你需要初始化大量复杂的库或框架，就需要管理其依赖关系并且按正确的顺序执行。此时就可以用一个外观类来统一处理这些依赖关系，以对其进行整合。

## 解决
外观模式和建造者模式很相似。两者的区别在于，外观模式是一种结构型模式，她的目的是将对象组合起来，而不是像建造者模式那样创建出不同的产品。

请看以下代码
```go
package facade

import "fmt"

// 初始化APIA和APIB

type APIA interface {
  TestA() string
}

func NewAPIA() APIA {
  return &apiRunA{}
}

type apiRunA struct{}

func (*apiRunA) TestA() string {
  return "A api running"
}

type APIB interface {
  TestB() string
}

func NewAPIB() APIB {
  return &apiRunB{}
}

type apiRunB struct{}

func (*apiRunB) TestB() string {
  return "B api running"
}

// 外观类

type API interface {
  Test() string
}

func NewAPI() API {
  return &apiRun{
    a: NewAPIA(),
    b: NewAPIB(),
  }
}

type apiRun struct {
  a APIA
  b APIB
}

func (a *apiRun) Test() string {
  aRet := a.a.TestA()
  bRet := a.b.TestB()
  return fmt.Sprintf("%s\n%s", aRet, bRet)
}
```
假设要初始化APIA和APIB，我们就可以通过一个外观类API进行处理，在外观类接口Test方法中分别执行类TestA方法和TestB方法。

测试代码：
```go
package facade

import "testing"

var expect = "A api running\nB api running"

// TestFacadeAPI ...
func TestFacadeAPI(t *testing.T) {
  api := NewAPI()
  ret := api.Test()
  if ret != expect {
    t.Fatalf("expect %s, return %s", expect, ret)
  }
}
```