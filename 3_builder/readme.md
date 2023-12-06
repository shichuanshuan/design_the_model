# ������ģʽ Builder
## ����
����ҵ����Ҫ�����贴��һϵ�и��ӵĶ���ʵ����Щ����Ĵ������һ��ǳ����������ǿ��Խ���Щ����Ž�һ���������ڶ�����Ĺ��캯���У���������캯������������ǳ��������£�������ά����

����ҵ����Ҫ����һ�����Ӷ�����Ҫ�ȴ�ػ�����ǽ�����ݶ�������԰�����üҾߡ�����������Ҫ�ǳ���Ĳ��裬������Щ����֮��������ϵ�ģ���ʹ�����������һ����Ĺ��캯�����������С�����У���������Ĳ�νṹ��������Ȼ�ܸ��ӡ�

��ν���أ������ָ��ӵ�����ಽ��Ĺ��캯�����Ϳ����ý�����ģʽ����ơ�

������ģʽ���ô��������ܹ��ֲ��贴�����Ӷ���

## ���
�ڽ�����ģʽ�У�������Ҫ�����Ķ���ÿ������Ĵ��룬Ȼ����һ�����캯���в�����Щ���裬������Ҫһ�������࣬���������������������衣�������Ǿ�ֻ��Ҫ�������������һ�����캯�������캯���ٽ��������ݸ���Ӧ�������࣬�������������ɺ������н�������

�뿴���´��룺
```go
package builder

import "fmt"

// �����߽ӿ�
type Builder interface {
  Part1()
  Part2()
  Part3()
}

// ������
type Director struct {
  builder Builder
}

// ���캯��
func NewDirector(builder Builder) *Director {
  return &Director{
    builder: builder,
  }
}

// ����
func (d *Director) Construct() {
  d.builder.Part1()
  d.builder.Part2()
  d.builder.Part3()
}

type Builder struct {}

func (b *Builder) Part1() {
  fmt.Println("part1")
}

func (b *Builder) Part2() {
  fmt.Println("part2")
}

func (b *Builder) Part3() {
  fmt.Println("part3")
}
```
���ϣ�����ʵ��part1��part2��part3��3�����裬ֻ��Ҫִ�й��캯������Ӧ�Ĺ�����Ϳ������н��췽��Construct�����3�������ִ�С�

���Դ���:
```go
package builder

func ExampleBuilder() {
  builder := &Builder{}
  director := NewDirector(builder)
  director.Construct()
  // Output:
  // part1
  // part2
  // part3
}
```