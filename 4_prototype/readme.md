# ԭ��ģʽ Prototype
## ����
�����ϣ������һ������������һ��������ȫ��ͬ�������ʵ���أ�

���������������г�Ա���������θ��Ƶ��¶����У��������鷳��������Щ������ܻ���˽�г�Ա������©��

ԭ��ģʽ�������¡�Ĺ���ί�ɸ��˱���¡��ʵ�ʶ��󣬱���¡�Ķ���ͽ�����ԭ�͡���

## ���
�����Ҫ��¡һ���µĶ������������ȫ����������ԭ�ͣ���ô�Ϳ���ʹ��ԭ��ģʽ��

ԭ��ģʽ��ʵ�ַǳ��򵥣��뿴���´��룺
```go
package prototype

import "testing"

var manager *PrototypeManager

type Type1 struct {
  name string
}

func (t *Type1) Clone() *Type1 {
  tc := *t
  return &tc
}

func TestClone(t *testing.T) {
  t1 := &Type1{
    name: "type1",
  }

  t2 := t1.Clone()

  if t1 == t2 {
    t.Fatal("error! get clone not working")
  }
}
```
��������һ��Clone����ʵ����ԭ��Type1�Ŀ�¡��

ԭ��ģʽ���ô����������ǿ��Կ�¡���󣬶�������ԭ�Ͷ������������ϡ�