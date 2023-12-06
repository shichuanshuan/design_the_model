# ��Ԫģʽ Flyweight
## ����
��һЩ����£�����û���㹻���ڴ�����֧�ִ洢�������󣬻��ߴ����Ķ���洢���ظ���״̬����ʱ�ͻ�����ڴ���Դ���˷ѡ�

��Ԫģʽ����������Ľ����������������������ͬ��״̬���Թ��ã������������޵��ڴ�����������������

## ���
������˵����Ԫģʽϣ����ȡ�����ڶ������乲����ظ�״̬��

���ǿ���ʹ��map�ṹ��ʵ����һ���룬������Ҫ�洢һЩ������ɫ�Ķ���ʹ����Ԫģʽ�������������뿴���´��룺
```go
package flyweight

import "fmt"

// ��Ԫ����

type ColorFlyweightFactory struct {
  maps map[string]*ColorFlyweight
}

var colorFactory *ColorFlyweightFactory

func GetColorFlyweightFactory() *ColorFlyweightFactory {
  if colorFactory == nil {
    colorFactory = &ColorFlyweightFactory{
      maps: make(map[string]*ColorFlyweight),
    }
  }
  return colorFactory
}

func (f *ColorFlyweightFactory) Get(filename string) *ColorFlyweight {
  color := f.maps[filename]
  if color == nil {
    color = NewColorFlyweight(filename)
    f.maps[filename] = color
  }

  return color
}

type ColorFlyweight struct {
  data string
}

// �洢color����

func NewColorFlyweight(filename string) *ColorFlyweight {
  // Load color file
  data := fmt.Sprintf("color data %s", filename)
  return &ColorFlyweight{
    data: data,
  }
}

type ColorViewer struct {
  *ColorFlyweight
}

func NewColorViewer(name string) *ColorViewer {
  color := GetColorFlyweightFactory().Get(name)
  return &ColorViewer{
    ColorFlyweight: color,
  }
}
```
���Ƕ�����һ����Ԫ������ʹ��map�洢��ͬ����key����״̬��value���������Ԫ��������ʹ���Ǹ�����Ͱ�ȫ�ķ��ʸ�����Ԫ����֤��״̬�����޸ġ�

���Ƕ�����NewColorViewer���������������Ԫ������Get�����洢���󣬶�����Ԫ������ʵ���п��Կ�������ͬ״̬�Ķ���ֻ��ռ��һ�Ρ�

���Դ��룺
```go
package flyweight

import "testing"

func TestFlyweight(t *testing.T) {
  viewer1 := NewColorViewer("blue")
  viewer2 := NewColorViewer("blue")

  if viewer1.ColorFlyweight != viewer2.ColorFlyweight {
    t.Fail()
  }
}
```
��������Ҫ�洢����������û���㹻���ڴ�����ʱ�����Կ���ʹ����Ԫģʽ��