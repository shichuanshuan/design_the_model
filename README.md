# design_the_model
 23 种设计模式 go 语言实现示例

# 参考链接
https://blog.csdn.net/Klaus_S/article/details/130615962

[toc]
## 创建型模式
```
创建型模式是处理对象创建的设计模式，试图根据实际情况使用合适的方式创建对象，增加已有代码的灵活性和可复用性
```
### 工厂方法模式 Factory Method

### 抽象工厂模式 Abstract Factory

### 建造者模式 Builder

### 原型模式 Prototype

### 单例模式 Singleton

## 结构型模式
```
结构型模式将一些对象和类组装成更大的结构体，并同时保持结构的灵活和高效。
```
### 适配器模式 Adapter

### 桥接模式Bridge

### 对象树模式Object Tree

### 装饰模式Decorator

### 外观模式Facade

### 享元模式 Flyweight

### 代理模式Proxy

## 行为型模式
```
行为型模式处理对象和类之间的通信，并使其保持高效的沟通和委派。
```
### 责任链模式Chain of Responsibility

### 命令模式Command

### 迭代器模式Iterator

### 中介者模式Mediator

### 备忘录模式Memento

### 观察者模式Observer

### 状态模式 State

### 策略模式Strategy

### 模板方法模式Template Method

### 访问者模式Visitor




设计模式的“道”
开闭原则
里氏替换原则
依赖倒置原则
单一职责原则
最少知道原则
接口分离原则

设计模式的“道”
上面那么多种设计模式你能记住几种呢？设计模式分为“术”的部分和“道”的部分，上面那些设计模式就是“术”的部分，他们是一些围绕着设计模式核心思路的经典解决方案。换句话说，重要的是理解为什么要用那些设计模式，具体问题，具体分析，而不是把某种设计模式生搬硬套进代码。

设计模式有6大原则，以上的设计模式目的就是为了使软件系统能达到这些原则：

开闭原则
软件应该对扩展开放，对修改关闭。

对系统进行扩展，而无需修改现有的代码。这可以降低软件的维护成本，同时也增加可扩展性。

里氏替换原则
任何基类可以出现的地方，子类一定可以出现。

里氏替换原则是对开闭原则的补充，实现开闭原则的关键步骤就是抽象化，基类与子类的关系就是要尽可能的抽象化。

依赖倒置原则
面向接口编程，抽象不应该依赖于具体类，具体类应当依赖于抽象。

这是为了减少类间的耦合，使系统更适宜于扩展，也更便于维护。

单一职责原则
一个类应该只有一个发生变化的原因。

一个类承载的越多，耦合度就越高。如果类的职责单一，就可以降低出错的风险，也可以提高代码的可读性。

最少知道原则
一个实体应当尽量少地与其他实体之间发生相互作用。

还是为了降低耦合，一个类与其他类的关联越少，越易于扩展。

接口分离原则
使用多个专门的接口，而不使用高耦合的单一接口。

避免同一个接口占用过多的职责，更明确的划分，可以降低耦合。高耦合会导致程序不易扩展，提高出错的风险。