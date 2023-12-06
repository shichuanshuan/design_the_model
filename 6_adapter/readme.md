# ������ģʽ Adapter
## ����
������ģʽ˵���˾��Ǽ��ݡ�

����һ��ʼ�����ṩ��A���󣬺�������ҵ�����������Ҫ��A����Ļ���֮����������ͬ����������кܶຯ���Ѿ������ϵ�����A���󣬴�ʱ�ٶ�A��������޸ľͱȽ��鷳����Ϊ��Ҫ���Ǽ������⡣���и���������� �����û�г�����Դ���룬 �Ӷ��޷���������޸ġ�

��ʱ�Ϳ�����һ����������������һ���ӿ�ת���������÷�ֻ��Ҫ��������������ӿڣ�������Ҫ��ע�䱳���ʵ�֣����������ӿڷ�װ���ӵĹ��̡�

## ���
������2���ӿڣ�һ��������תΪ�ף�һ������תΪ���ס������ṩһ���������ӿڣ�ʹ���÷�����Ҫ�ٲ��ĵ����ĸ��ӿڣ�ֱ�������������ü��ݡ�

�뿴���´��룺
```go
package adapter

// �ṩһ����ȡ�׵Ľӿں�һ����ȡ���׵Ľӿ�

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

// ������

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
����ʵ����Cm��M�����ӿڣ�����������LengthAdapter�����ݡ�

���Դ��룺
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