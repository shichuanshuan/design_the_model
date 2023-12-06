# 模板方法模式Template Method
## 问题
模板方法模式就是将算法分解为一系列步骤，然后在一个模版方法中依次调用这些步骤。这样客户端就不需要了解各个步骤的实现细节，只需要调用模版即可。

## 解决
一个非常简单的例子，请看以下代码：
```go
package templatemethod

import "fmt"

type PrintTemplate interface {
  Print(name string)
}

type template struct {
  isTemplate PrintTemplate
  name       string
}

func (t *template) Print() {
  t.isTemplate.Print(t.name)
}

type A struct{}

func (a *A) Print(name string) {
  fmt.Println("a: " + name)
  // 业务代码……
}

type B struct{}

func (b *B) Print(name string) {
  fmt.Println("b: " + name)
  // 业务代码……
}
```
测试代码：
```go
package templatemethod

func ExamplePrintTemplate() {
	templateA := &A{}
	template := &template{
		isTemplate: templateA,
		name:       "hi~",
	}
	template.Print()

	templateB := &B{}
	template.isTemplate = templateB
	template.Print()

	// Output:
	// a: hi~
	// b: hi~
}
```