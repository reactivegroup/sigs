# cdubbo的设计思路
## dubbo设计
![dubbo-framework](pic/dubbo-framework.jpg)

## cdubbo设计
### 嵌入dubbo的思路
通过实现dubbo中不同层次的接口，将cdubbo的实现类嵌入到dubbo流程中。
同时cdubbo的实现类持有自己特有的资源（初始化流程中的资源），在调用流程中使用自己持有的资源做特定逻辑

### 层次设计思路
#### 动静结合
> （与Tomcat的mapper层设计思路相似）
cdubbo通过实现类将自身伪装成invoker等，注入到dubbo的调用流程中，但这些实现类并不直接持有资源。
资源的持有交给同层级的Manager，通过静态域管理该层次的资源。
在实现类执行动作的时候，将会从Manager静态资源管理器中，获取资源。

#### 资源插件化
Manager静态资源管理器，使用插件化的方式，在初始化的时候可以装配不同的插件，从而加载不同的资源

#### cdubbo组件
##### Filter
Hystrix：断路器功能

##### Registry
Artemis：注册中心

##### Router
Artemis：注册中心

##### Other
接入堡垒平台，用作堡垒测试
接入Cat，进行异常的打点
接入SOA，通过http直接访问soa服务