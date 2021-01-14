# Message 消息
“消息驱动系统”的概念是Actor模型最基本的概念之一，这是由[响应式宣言](http://www.reactivemanifesto.org/glossary#Message-Driven)定义的：

> 消息是发送到特定目标的数据项。
事件是组件在满足给定状态时发出的信号。
在消息驱动的系统中，可寻址的收件人等待消息的到达，并对消息做出反应，否则，它将处于休眠状态。

Proto.Actor中的actor使用消息传递的方式来互相通信。

## 如何定义消息

在Proto.Actor中，消息就是个简单的对象:

```go
// go
type MyMessage struct {
    Name string
}
```

Proto.Actor允许您将这些消息传递给任何actor，无论它是在应用程序本地进程中运行的actor还是在另一台计算机上运行的远程actor。 
Proto.Actor可以自动序列化您的消息并将其路由到其期望的收件人。

## actor根据消息内容更改其内部状态

actor的定义特征之一是，它们用线程安全的方式更改自身状态的能力，并且基于收到的消息内容来执行此操作。


这是一个简单的示例：

```go
import (
    "fmt"
    "github.com/AsynkronIT/protoactor-go/actor"
)

type MyMessage struct{
    Name string
}

type Hi struct{}

type MyActor struct{
    lastActorName string
}

func (a *MyActor) Receive(c actor.Context){
    switch msg := c.Message().(type)
    case *MyMessage:
        a.lastActorName = msg.Name
    case *Hi:
        fmt.Println("Hi ", a.lastActorName)
}


func main() {
    sys := actor.NewActorSystem()
    rootContext := sys.Root
    pid := rootContext.Spawn(actor.PropsFromProducer(func()actor.Actor{return &MyActor}))

    rootContext.Send(pid, &MyMessage{Name:"Hello world"})
    rootContext.Send(pid, &Hi{})
}
```

每当MyActor.lastActorName收到一个MyMessage，它便会将其中的Name设置为自己的Name，每当收到Hi消息类型时，则会打印最新的Name到控制台。


您可以通过传递消息来修改actor的可变状态。


## 消息是不可变的

不可变的消息本质上是线程安全的。
没有线程可以修改不可变消息的内容，因此接收原始消息的第二个线程不必担心前一个线程以无法预测的方式更改状态。


因此，在Proto.Actor中，所有消息都应该是不可变的，线程安全的。
这就是为什么我们可以拥有成千上万个Proto.Actor actor并发地处理消息而没有同步机制的原因之一，因为不可变的消息消除了这一问题。


## 消息传递是异步的

在OOP中，您的对象通过函数调用相互通信。
过程编程也是如此。 
A类在B类上调用一个函数，并等待该函数返回，然后A类才能继续进行其余工作。


在Proto.Actor和Actor模型中，actor是通过发送消息来相互通信的。


那么，这个想法有何妙之处？


消息传递是异步的--发送消息的actor可以继续执行其他工作，而接收actor则会处理发送者的消息。
默认情况下，一个actor与其他actor之间的每次交互都是异步的。


这是一个巨大的变化，但还有另外一个巨大的变化……


由于所有“函数调用”已被消息（即对象的不同实例）替换，因此actor可以存储其函数调用的历史记录，甚至可以将处理某些函数调用的时间推迟到以后！


想象一下，使用actor在Microsoft Word中构建诸如“撤消”按钮之类的东西会多么容易-默认情况下，您会收到一条消息，该消息代表某人对文档所做的每项更改。
要撤消其中的一项更改，您只需将消息从UndoActor的消息存储库中弹出，然后将所做的更改推回到管理Word文档当前状态的另一个actor。
实际上，这是一个非常强大的概念。