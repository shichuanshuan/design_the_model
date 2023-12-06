# ״̬ģʽ State
## ����
���һ�������ʵ�ַ�������������״̬���ı䣬�Ϳ���ʹ��״̬ģʽ��

�ٸ����ӣ�������һ�����ŵķ������ŵ�״̬��һ��ʼ�ǡ��رա��������ִ��open������close����������ִ����open�������ŵ�״̬�ͱ���ˡ�����������ִ��open�����Ͳ���ִ�п��ŵĹ��ܣ����Ƿ��ء����ѿ����������ִ��close�������ŵ�״̬�ͱ���ˡ��رա�����ִ��close�����Ͳ���ִ�й��ŵĹ��ܣ����Ƿ��ء����ѹرա�������һ���򵥵����ӣ����ǽ�Ϊÿ��״̬�ṩ��ͬ��ʵ�ַ���������Щ������֯�������鷳�����״̬ҲԽ��Խ���أ����ɣ��⽫��ʹ������ӷ�ס�

## ���
���������ҪΪһ���Ŷ����ṩ3��״̬�µ�open��close������

* ��������״̬�£�open�������ء����ѿ�������close�������ء��رճɹ�����
* ���رա�״̬�£�open�������ء������ɹ�����close�������ء����ѹرա���
* ���𻵡�״̬�£�open�������ء������𻵣��޷���������close�������ء������𻵣��޷��رա���
�뿴���´��룺
```go
package state

import "fmt"

// ��ͬ״̬��Ҫʵ�ֵĽӿ�

type state interface {
  open(*door)
  close(*door)
}

// �Ŷ���

type door struct {
  opened  state
  closed  state
  damaged state

  currentState state // ��ǰ״̬
}

func (d *door) open() {
  d.currentState.open(d)
}

func (d *door) close() {
  d.currentState.close(d)
}

func (d *door) setState(s state) {
  d.currentState = s
}

// ����״̬

type opened struct{}

func (o *opened) open(d *door) {
  fmt.Println("���ѿ���")
}

func (o *opened) close(d *door) {
  fmt.Println("�رճɹ�")
}

// �ر�״̬

type closed struct{}

func (c *closed) open(d *door) {
  fmt.Println("�����ɹ�")
}

func (c *closed) close(d *door) {
  fmt.Println("���ѹر�")
}

// ��״̬

type damaged struct{}

func (a *damaged) open(d *door) {
  fmt.Println("�����𻵣��޷�����")
}

func (a *damaged) close(d *door) {
  fmt.Println("�����𻵣��޷��ر�")
}
```
���ǵ��Ŷ���doorʵ����open��close�������ڷ����У�ֻ��Ҫ���õ�ǰ״̬currentState��open��close�������ɡ�

���Դ��룺
```go
package state

func ExampleState() {
  door := &door{}

  // ����״̬
  opened := &opened{}
  door.setState(opened)
  door.open()
  door.close()

  // �ر�״̬
  closed := &closed{}
  door.setState(closed)
  door.open()
  door.close()

  // ��״̬
  damaged := &damaged{}
  door.setState(damaged)
  door.open()
  door.close()

  // Output:
  // ���ѿ���
  // �رճɹ�
  // �����ɹ�
  // ���ѹر�
  // �����𻵣��޷�����
  // �����𻵣��޷��ر�
}
```