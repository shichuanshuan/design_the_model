# ģ�巽��ģʽTemplate Method
## ����
ģ�巽��ģʽ���ǽ��㷨�ֽ�Ϊһϵ�в��裬Ȼ����һ��ģ�淽�������ε�����Щ���衣�����ͻ��˾Ͳ���Ҫ�˽���������ʵ��ϸ�ڣ�ֻ��Ҫ����ģ�漴�ɡ�

## ���
һ���ǳ��򵥵����ӣ��뿴���´��룺
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
  // ҵ����롭��
}

type B struct{}

func (b *B) Print(name string) {
  fmt.Println("b: " + name)
  // ҵ����롭��
}
```
���Դ��룺
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