# 适配器模式 Adapter
## 问题
适配器模式说白了就是兼容。

假设一开始我们提供了A对象，后期随着业务迭代，又需要从A对象的基础之上衍生出不同的需求。如果有很多函数已经在线上调用了A对象，此时再对A对象进行修改就比较麻烦，因为需要考虑兼容问题。还有更糟糕的情况， 你可能没有程序库的源代码， 从而无法对其进行修改。

此时就可以用一个适配器，它就像一个接口转换器，调用方只需要调用这个适配器接口，而不需要关注其背后的实现，由适配器接口封装复杂的过程。

## 解决
假设有2个接口，一个将厘米转为米，一个将米转为厘米。我们提供一个适配器接口，使调用方不需要再操心调用哪个接口，直接由适配器做好兼容。

请看以下代码：
```go
package adapter

// 提供一个获取米的接口和一个获取厘米的接口

type Cm interface {
  getLength(float64) float64
}

type M interface {
  getLength(float64) float64
}

func NewM() M {
  return &getLengthM{}
}

type getLengthM struct{}

func (*getLengthM) getLength(cm float64) float64 {
  return cm / 10
}

func NewCm() Cm {
  return &getLengthCm{}
}

type getLengthCm struct{}

func (a *getLengthCm) getLength(m float64) float64 {
  return m * 10
}

// 适配器

type LengthAdapter interface {
  getLength(string, float64) float64
}

func NewLengthAdapter() LengthAdapter {
  return &getLengthAdapter{}
}

type getLengthAdapter struct{}

func (*getLengthAdapter) getLength(isType string, into float64) float64 {
  if isType == "m" {
    return NewM().getLength(into)
  }
  return NewCm().getLength(into)
}
```
上面实现了Cm和M两个接口，并由适配器LengthAdapter做兼容。

测试代码：
```go
package adapter

import "testing"

func TestAdapter(t *testing.T) {
	into := 10.5
	getLengthAdapter := NewLengthAdapter().getLength("m", into)
	getLengthM := NewM().getLength(into)
	if getLengthAdapter != getLengthM {
		t.Fatalf("getLengthAdapter: %f, getLengthM: %f", getLengthAdapter, getLengthM)
	}
}
```