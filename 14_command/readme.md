# ����ģʽCommand
## ����
������ʵ���˿����͹رյ��ӻ��Ĺ��ܣ�����ҵ�����������Ҫʵ�ֿ����͹رձ���Ĺ��ܣ������͹رյ�ƵĹ��ܣ������͹ر�΢��¯�Ĺ��ܡ�����Щ���ܶ�������Ļ��࣬�����͹رա������֮��Ի�������޸ģ��ܿ��ܻ�Ӱ�쵽�������ܣ���ʹ��Ŀ��ò��ȶ��ˡ�

һ�����������������ע������ķֲ���������ģʽ��ͼ���������Ľ����������Ͷ�Ӧ���ܽ�����ܸ��ݲ�ͬ�������䷽����������

## ���
�����ÿ����͹رռ��õ����������������ɡ��뿴���´���
```go
package command

import "fmt"

// ������

type button struct {
  command command
}

func (b *button) press() {
  b.command.execute()
}

// ��������ӿ�

type command interface {
  execute()
}

type onCommand struct {
  device device
}

func (c *onCommand) execute() {
  c.device.on()
}

type offCommand struct {
  device device
}

func (c *offCommand) execute() {
  c.device.off()
}

// ������

type device interface {
  on()
  off()
}

type tv struct{}

func (t *tv) on() {
  fmt.Println("Turning tv on")
}

func (t *tv) off() {
  fmt.Println("Turning tv off")
}

type airConditioner struct{}

func (t *airConditioner) on() {
  fmt.Println("Turning air conditioner on")
}

func (t *airConditioner) off() {
  fmt.Println("Turning air conditioner off")
}
```
���Ƿֱ�ʵ����������button������ӿ�command��������device��������button�������Ǹ�����ִ�п�����رյ�ң����������ӿ�command����һ���м�㣬��ʹ���ǵ������ߺͽ����߽�ź��

���Դ��룺
```go
package command

func ExampleCommand() {
  Tv()
  AirConditioner()

  // Output:
  // Turning tv on
  // Turning tv off
  // Turning air conditioner on
  // Turning air conditioner off
}

func Tv() {
  tv := &tv{}

  onTvCommand := &onCommand{
    device: tv,
  }

  offTvCommand := &offCommand{
    device: tv,
  }

  onTvButton := &button{
    command: onTvCommand,
  }
  onTvButton.press()

  offTvButton := &button{
    command: offTvCommand,
  }
  offTvButton.press()
}

func AirConditioner() {
  airConditioner := &airConditioner{}

  onAirConditionerCommand := &onCommand{
    device: airConditioner,
  }

  offAirConditionerCommand := &offCommand{
    device: airConditioner,
  }

  onAirConditionerButton := &button{
    command: onAirConditionerCommand,
  }
  onAirConditionerButton.press()

  offAirConditionerButton := &button{
    command: offAirConditionerCommand,
  }
  offAirConditionerButton.press()
}
```