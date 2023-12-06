# ��������ģʽ Factory Method
## ����
�������ǵ�ҵ����Ҫһ��֧�����������ǿ�����һ��Pay���������������֧�����뿴����ʾ����
```go
type Pay interface {
  Pay() string
}

type PayReq struct {
  OrderId string // ������
}

func (p *PayReq) Pay() string {
  // todo
  fmt.Println(p.OrderId)
  return "֧���ɹ�"
}
```
���ϣ����Ƕ����˽ӿ�Pay����ʵ�����䷽��Pay()��

���ҵ������������Ҫ�����ṩ����֧����ʽ��һ�ֽ�APay��һ�ֽ�BPay�������֧����ʽ����Ĳ�����ͬ��APayֻ��Ҫ������OrderId��BPay����Ҫ������OrderId��Uid����ʱ����޸ģ�

�������뵽������ԭ�еĴ���������޸ģ����磺
```go
type Pay interface {
  APay() string
  BPay() string
}

type PayReq struct {
  OrderId string // ������
  Uid int64
}

func (p *PayReq) APay() string {
  // todo
  fmt.Println(p.OrderId)
  return "APay֧���ɹ�"
}

func (p *PayReq) BPay() string {
  // todo
  fmt.Println(p.OrderId)
  fmt.Println(p.Uid)
  return "BPay֧���ɹ�"
}
```
����ΪPay�ӿ�ʵ����APay() ��BPay() ��������Ȼ��ʱʵ����ҵ�����󣬵�ȴʹ�ýṹ��PayReq��������ˣ�APay() ������ҪUid���������֮��������CPay��DPay��EPay�������֪���������Խ��Խ����ά����

���ź���ҵ������������ò���д�����ӵĴ��롣

## ���
����������һ�������࣬�����������Ҫ�������ߺͿ��ص����ߣ����ǿ���Ϊ�������ṩһ�����������������߻���������������ʱ���Ͳ������ߣ������ػ���������������ʱ���Ͳ������ء�

���õ����ǵ�֧��ҵ�������������ǲ���Ϊ�ӿ��ṩAPay������BPay��������ֻ�ṩһ��Pay����������A֧����ʽ��B֧����ʽ�������·ŵ����ࡣ
```go
package factorymethod

import "fmt"

type Pay interface {
  Pay(string) int
}

type PayReq struct {
  OrderId string
}

type APayReq struct {
  PayReq
}

func (p *APayReq) Pay() string {
  // todo
  fmt.Println(p.OrderId)
  return "APay֧���ɹ�"
}

type BPayReq struct {
  PayReq
  Uid int64
}

func (p *BPayReq) Pay() string {
  // todo
  fmt.Println(p.OrderId)
  fmt.Println(p.Uid)
  return "BPay֧���ɹ�"
}
```
������APay��BPay�����ṹ����д��Pay() �����������Ҫ���һ���µ�֧����ʽ�� ֻ��Ҫ��д�µ�Pay() �������ɡ�

�����������ŵ�����ڱ����˴����ߺ;����Ʒ֮��Ľ�����ϣ��Ӷ�ʹ�ô��������ά����
```go
package factorymethod

import (
  "testing"
)

func TestPay(t *testing.T) {
  aPay := APayReq{}
  if aPay.Pay() != "APay֧���ɹ�" {
    t.Fatal("aPay error")
  }

  bPay := BPayReq{}
  if bPay.Pay() != "BPay֧���ɹ�" {
    t.Fatal("bPay error")
  }
}
```