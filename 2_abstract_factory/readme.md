# ���󹤳�ģʽ Abstract Factory
## ����
���󹤳�ģʽ���ڹ�������ģʽ�����ߵ��������ڣ���������ģʽ�Ǵ�����һ�ֲ�Ʒ�������󹤳�ģʽ�Ǵ�����һ���Ʒ������ֶ����ڹ���ģʽ��������������Ƶġ�

���裬��һ���洢�������ṩredis��mysql���ִ洢���ݵķ�ʽ�����ʹ�ù�������ģʽ�����Ǿ���Ҫһ���洢���������ṩSaveRedis������SaveMysql������

## ���
�����ĵĴ洢����ҵ��Ϊ�����ó��󹤳�ģʽ��˼·����ƴ��룬������������
```go
package abstractfactory

import "fmt"

// SaveArticle ����ģʽ�����ӿ�
type SaveArticle interface {
  CreateProse() Prose
  CreateAncientPoetry() AncientPoetry
}

type SaveRedis struct{}

func (*SaveRedis) CreateProse() Prose {
  return &RedisProse{}
}

func (*SaveRedis) CreateAncientPoetry() AncientPoetry {
  return &RedisProse{}
}

type SaveMysql struct{}

func (*SaveMysql) CreateProse() Prose {
  return &MysqlProse{}
}

func (*SaveMysql) CreateAncientPoetry() AncientPoetry {
  return &MysqlProse{}
}

// Prose ɢ��
type Prose interface {
  SaveProse()
}

// AncientPoetry ��ʫ
type AncientPoetry interface {
  SaveAncientPoetry()
}

type RedisProse struct{}

func (*RedisProse) SaveProse() {
  fmt.Println("Redis Save Prose")
}

func (*RedisProse) SaveAncientPoetry() {
  fmt.Println("Redis Save Ancient Poetry")
}

type MysqlProse struct{}

func (*MysqlProse) SaveProse() {
  fmt.Println("Mysql Save Prose")
}

func (*MysqlProse) SaveAncientPoetry() {
  fmt.Println("Mysql Save Ancient Poetry")
}
```
���Ƕ����˴洢������Ҳ����SaveArticle�ӿڣ���ʵ����CreateProse������CreateAncientPoetry��������2�������ֱ����ڴ���ɢ�Ĺ����͹�ʫ������

Ȼ�������ֱַ�Ϊɢ�Ĺ����͹�ʫ����ʵ����SaveProse������SaveAncientPoetry����������Redis�ṹ���Mysql�ṹ��ֱ���д��2�ִ洢������

���Դ��룺
```go
package abstractfactory

func Save(saveArticle SaveArticle) {
  saveArticle.CreateProse().SaveProse()
  saveArticle.CreateAncientPoetry().SaveAncientPoetry()
}

func ExampleSaveRedis() {
  var factory SaveArticle
  factory = &SaveRedis{}
  Save(factory)
  // Output:
  // Redis Save Prose
  // Redis Save Ancient Poetry
}

func ExampleSaveMysql() {
  var factory SaveArticle
  factory = &SaveMysql{}
  Save(factory)
  // Output:
  // Mysql Save Prose
  // Mysql Save Ancient Poetry
}

```