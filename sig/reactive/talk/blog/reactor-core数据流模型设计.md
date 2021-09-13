TODO 反应式编程

## 一、反应式编程库介绍

### 1.1 为什么要使用反应式编程库？

更好的写异步代码。

统一的异步编码方式。

链式调用。

### 1.2 反应式编程库示例

Mono Flux.

spring-webflux框架/vertx/quarkus

> 静态编译技术：graalVM/android

### 1.3 反应式编程库和其他异步编程框架的对比

不同的异步编程方式：

## 二、反应式编程库原理

### 2.1 有哪些反应式编程库

rxjava/reactor-core标准

### 2.2 反应式库的核心接口

Publisher/Subscriber/Subctiption

java9

### 2.3 实现原理概括

#### 1. 命令式编程

![reactor-设计1](pic/reactor-设计1.png)

#### 2. 声明式编程

![reactor-设计2](pic/reactor-设计2.png)

#### 3. 函数式编程Stream

去除中间过程，横向改为纵向处理

![reactor-设计3](pic/reactor-设计3.png)

##### 1. 声明阶段

将Op组装成Pipeline

##### 2. 回溯阶段

生成Sink链

##### 3. 执行阶段

执行Sink方法

> 最核心的就是类为AbstractPipeline，ReferencePipeline和Sink接口.AbstractPipeline抽象类是整个Stream中流水线的高度抽象了源头sourceStage，上游previousStage，下游nextStage，定义evaluate结束方法，而ReferencePipeline则是抽象了过滤,转换,聚合,归约等功能，每一个功能的添加实际上可以理解为卷心菜，菜心就是源头，每一次加入一个功能就相当于重新长出一片叶子包住了菜心，最后一个功能集成完毕之后整颗卷心菜就长大了。而Sink接口呢负责把整个流水线串起来，然后在执行聚合,归约时候调AbstractPipeline抽象类的evaluate结束方法，根据是否是并行执行，调用不同的结束逻辑，如果不是并行方法则执行terminalOp.evaluateSequential否则就执行terminalOp.evaluateParallel，非并行执行模式下则是执行的是AbstractPipeline抽象类的wrapAndCopyInto方法去调用copyInto，调用前会先执行一下wrapSink，用于剥开这个我们在流水线上产生的卷心菜。从下游向上游去遍历AbstractPipeline，然后包装到Sink，然后在copyInto方法内部迭代执行对应的方法。最后完成调用

#### 4. 反应式编程Reactor

![reactor-设计4](pic/reactor-设计4.png)

反应式编程模型在执行中主要有5条链路:

##### 1. 声明流程

组装reactor执行链路，绑定上下游节点，在 subscribe() 之前，我们什么都没做，只是在不断的包裹 Publisher 将作为原始的 Publisher 一层又一层的返回回来。

生成Publisher

##### 2. 回溯流程

`subscribe()`

生成Subscriber

> 内存数据发送很简单，就是循环发送。而对于像数据库、RPC 这样的场景，则会触发请求的发送。

> Publisher 接口中的 subscribe 方法语义上有些奇特，它表示的不是订阅关系，而是被订阅关系。即 aPublisher.subscribe(aSubscriber) 表示的是 aPublisher 被 aSubscriber 订阅。

> 对于中间过程的 Mono/Flux，subscribe 阶段是订阅上一个 Mono/Flux；而对于源 Mono/Flux，则是要执行 Subscriber.onSubscribe(Subscription s) 方法。

##### 3. 通知流程

`onSubscribe()`

生成Subscription，并将Subscriber作为成员参数传入

##### 4. 背压流程 

`request(n)`

基于Subscription实现request(n)机制

##### 5. 执行流程

`doNext()`/....

基于Subscription中的Subscriber实现执行机制

![reaactor-core流程](pic/reaactor-core流程.png)

## 三、Reactor源码分析

### 3.1 Reactor-core工作原理

![reactor](https://s1.ax1x.com/2018/06/27/PPboXq.png)

### SPI 模型定义

#### Publisher 即被观察者

Publisher 在 PRR 中 所承担的角色也就是传统的 观察者模式 中的 被观察者对象，在 PRR 的定义也极为简单。

```java
package org.reactivestreams;
public interface Publisher<T> {
    public void subscribe(Subscriber<? super T> s);
}
```

Publisher 的定义可以看出来，Publisher 接受 Subscriber，非常简单的一个接口。但是这里有个有趣的小细节，这个类所在的包是 org.reactivestreams，这里的做法和传统的 J2EE 标准类似，我们使用标准的 Javax 接口定义行为，不定义具体的实现。

#### Subscriber 即观察者

Subscriber 在 PRR 中 所承担的角色也就是传统的 观察者模式 中的 观察者对象，在 PRR 的定义要多一些。

```java
public interface Subscriber<T> {
    public void onSubscribe(Subscription s); ➊
    public void onNext(T t); ➋
    public void onError(Throwable t); 
    public void onComplete(); ➍
}
```

➊ 订阅时被调用
➋ 每一个元素接受时被触发一次
➌ 当在触发错误的时候被调用
➍ 在接受完最后一个元素最终完成被调用

Subscriber 的定义可以看出来，Publisher 是主要的行为对象，用来描述我们最终的执行逻辑。

> 行为是由Subscriber触发的，这是一种Pull模型，如果是Publisher触发，则为Push模型。

#### Subscription 桥接者

在最基础的 观察者模式 中，我们只是需要 Subscriber 观察者 Publisher 发布者，而在 PRR 中增加了一个 Subscription 作为 Subscriber Publisher 的桥接者。

```java
public interface Subscription {
    public void request(long n); ➊
    public void cancel(); ➋
}
```

➊ 获取 N 个元素往下传递
➋ 取消执行

为什么需要这个对象，笔者觉得是一是为了解耦合，第二在 Reference 中有提到 Backpressure 也就是下游可以保护自己不受上游大流量冲击，这个在 Stream 编程中是无法做到的，想要做到这个，就需要可以控制流速，那秘密看起来也就是在 request(long n) 中。

> 中间层，主要用于解耦和流控。

### 3.2 实现细节

1. 声明阶段: 当我们每进行一次 Operator 操作 （也就 map filter flatmap），就会将原有的 FluxPublisher 包裹成一个新的 FluxPublisher

![声明](https://s1.ax1x.com/2018/06/29/PiLJN8.png)

最后生成的对象是这样的

![声明结果](https://s1.ax1x.com/2018/06/29/PiLCc9.png)

2. subscribe阶段: 当我们最终进行 subscribe 操作的时候，就会从最外层的 Publisher 一层一层的处理，从这层将 Subscriber 变化成需要的 Subscriber 直到最外层的 Publisher

![执行](https://s1.ax1x.com/2018/06/29/PiLN9g.png)

最后生成的对象是这样的

![执行结果](https://s1.ax1x.com/2018/06/29/PiOBxH.png)

3. onSubscribe阶段: 在最外层的 Publisher 的时候调用 上一层 Subscriber 的 onSubscribe 函数，在此处将 Publisher 和 Subscriber 包裹成一个 Subscription 对象作为 onSubscribe 的入参数。

![执行2](https://s1.ax1x.com/2018/06/29/PivrM4.md.png)

4. 最终在 原始 Subscriber 对象调用 request() ，触发 Subscription 的 Source 获得数据作为 onNext 的参数，但是注意 Subscription 包裹的是我们封装的 Subscriber 所有的数据是从 MapSubscriber 进行一次转换再给我们的原始 Subscriber 的。

![执行3](https://s1.ax1x.com/2018/06/29/PixEWT.md.png)

经过一顿分析，整个 PRR 是如何将操作整合起来的，我们已经有一个大致的了解，通过不断的包裹出新的 Subscriber 对象，在最终的 request() 行为中触发整个消息的处理，这个过程非常像 俄罗斯套娃，一层一层的将变化组合形变操作变成一个新的 Subscriber， 然后就和一个管道一样，一层一层的往下传递。

5. 最终在 Subscription 开始了我们整个系统的数据处理

![执行4](https://s1.ax1x.com/2018/06/29/PixelF.png)

### 3.3 Reactor示例/高级用法

### flatMap异步原理

map： 用于同步非阻塞的一对一转换

flatMap： 适用于异步非阻塞的1到N转换

demo：

1. 异步编程示例demo

2. 通过map模拟flatmap功能

3. 把对publisher的处理封装起来->flatmap

原理：

1. MonoCompletionStage，对异步对象的封装

2. MonoFlatMap，

在subscribe时直接调用下游的onsubscribe
在异步回调时调用上游的subscribe

## 四、总结

1. 在声明阶段，我们像 俄罗斯套娃 一样，创建一个嵌套一个的 Publisher
2. 在 subscribe() 调用的时候，我们从最外围的 Publisher 根据包裹的 Operator 创建各种 Subscriber
3. Subscriber 通过 onSubscribe() 使得他们像一个链条一样关联起来，并和 最外围的 Publisher 组合成一个 Subscription
4. 在最底层的 Subscriber 调用 onSubscribe 去调用 Subscription 的 request(n); 函数开始操作
5. 元素就像水管中的水一样挨个 经过 Subscriber 的 onNext()，直至我们最终消费的 Subscriber