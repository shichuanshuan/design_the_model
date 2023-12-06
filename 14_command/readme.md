# 命令模式Command
## 问题
假设你实现了开启和关闭电视机的功能，随着业务迭代，还需要实现开启和关闭冰箱的功能，开启和关闭电灯的功能，开启和关闭微波炉的功能……这些功能都基于你的基类，开启和关闭。如果你之后对基类进行修改，很可能会影响到其他功能，这使项目变得不稳定了。

一个优秀的设计往往会关注于软件的分层与解耦，命令模式试图做到这样的结果：让命令和对应功能解耦，并能根据不同的请求将其方法参数化。

## 解决
还是用开启和关闭家用电器的例子来举例吧。请看以下代码
```go
package command

import "fmt"

// 请求者

type button struct {
  command command
}

func (b *button) press() {
  b.command.execute()
}

// 具体命令接口

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

// 接收者

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
我们分别实现了请求者button，命令接口command，接收者device。请求者button就像是那个可以执行开启或关闭的遥控器，命令接口command则是一个中间层，它使我们的请求者和接收者解藕。

测试代码：
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