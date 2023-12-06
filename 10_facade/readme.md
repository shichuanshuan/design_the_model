# ���ģʽFacade
## ����
�������Ҫ��ʼ���������ӵĿ���ܣ�����Ҫ������������ϵ���Ұ���ȷ��˳��ִ�С���ʱ�Ϳ�����һ���������ͳһ������Щ������ϵ���Զ���������ϡ�

## ���
���ģʽ�ͽ�����ģʽ�����ơ����ߵ��������ڣ����ģʽ��һ�ֽṹ��ģʽ������Ŀ���ǽ��������������������������ģʽ������������ͬ�Ĳ�Ʒ��

�뿴���´���
```go
package facade

import "fmt"

// ��ʼ��APIA��APIB

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

// �����

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
����Ҫ��ʼ��APIA��APIB�����ǾͿ���ͨ��һ�������API���д����������ӿ�Test�����зֱ�ִ����TestA������TestB������

���Դ��룺
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