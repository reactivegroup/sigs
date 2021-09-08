# sigs

[![Gitter Chat](https://badges.gitter.im/Join%20Chat.svg)](https://groups.google.com/g/reactive-group)

SIG工作内容资料库

## 常见问题

#### 可以加入几个小组？

没有限制，看自己的兴趣和要求。

#### 怎么报名参与？

一切合作都在github上，找到对应的issue，在下面报名/交流/回复/讨论即可。

#### 从哪里获取咨询？

一是关注github上的动态；二是加入对应sig的trippal群。

#### 如何成为正式成员？

首先报名参与，然后持续的参与和贡献，积极的同学我们会邀请您加入我们正式成员，并赋予你相应的权限和荣誉。

## 工作流程

SIG的工作内容会挂在该项目的Issues当中，通过`Label`标识进行类别划分，成员可以自行查看和交流讨论。

若有意向完成某个工作内容，可以在对应Issue下进行回复，并在Issue当中进行后续的讨论和进度追踪。

当一个工作内容被领取后，会`Assignees`给指定成员，但其他成员仍可参与。

为了更好的进行类别划分，工作内容Issues命名一般为：

```
[SIG名称][工作内容] 描述

例如：[Cloud][Project] Capa configuration API
```

#### Label标识

+ `reactive`: reactive sig
+ `rpc`: rpc sig
+ `cloud`: cloud sig
+ `talk`: 技术分享
+ `work`: 技术项目
+ `help wanted`: 待领取
+ `processing`: 处理中

## 工作内容

### 每个SIG主要分为3块工作内容：

### 1. 技术分享

> 由于部分成员之前已经进行过几轮的技术分享，在对应领域已经有一定的技术储备。而有一部分刚加入的成员可能才刚刚接触，为了更好的满足各自的诉求，所以划分为两个分享方向。

**目前每个SIG设置两个技术分享：**

+ 分享1：面向刚刚加入的成员，借鉴成员之前的经验，进行对应领域的快速入门学习；
+ 分享2：面向有深入了解的成员，进行更深一步的学习和探究；

**分享内容分工：**

+ 每个分享方向划分为4~8个分享主题，该方向的成员可以自行选取主题（先选先得），选取后成为该主题的主讲人，分享前需要撰写一篇技术文章，每次分享由主讲人依据技术文章进行主题分享。

+ 如果只想参与听讲，也需要撰写一篇技术文章。

**技术文章要求：**

一个系列（4~8讲）提供一篇，主题需要与系列相关，主题自选内容自选，对质量没有要求。

时间上，在系列结束之前提交都可。

> 技术文章可以发布到该仓库的指定目录下。
> 
> 写的优秀的技术文章，后续可以发布到相关的技术公众号上，这是一个很好的个人品牌宣传的机会。

**技术分享报名：**

找到对应技术分享的Issue（下方链接），按照格式进行回复即可。

如果想做主讲人，需要提供分享的主题（后续可以视情况自己更改）。

如果只讲听讲，分享主题不用填，但记得要写技术博客。

**分享时间：**

每个分享方向两周进行一次分享，按顺序由一位主讲成员进行分享。

+ 分享1：单数周举行一次
+ 分享2：双数周举行一次

### 2. 技术项目

每个SIG会设置一个或者多个技术项目。

对于每个技术项目，会对其进行介绍和模块分工，并发布到Issues中。

成员可以在Issues中选取并领取任务，并在Issue当中进行后续的交流和进度追踪。

后续交流采用不定期的项目交流会，以及日常在github上进行交流。

**参与方式：**

可以在issue上进行回复交流，让其他同学了解到你的动态。

有想法或者想参与的部分，可以在issues上交流，或者新开一个issue。
 
> 自愿参与，参与的同学会不定期内部进行交流。相关资料也在参与的同学中进行分享。

### 3. 其他事务

其他需要维护的工作，例如资料整理、技术文章迁移等。

任务会发布到Issues当中，有兴趣的成员可以自行领取。

**领取方式：**

在相关issue下进行回复即可，回复你想做的内容。

---

## SIG工作内容大纲

> 报名、详情、进度和讨论等，点击链接查看对应Issue

## Reactive SIG

小组信息同步：https://github.com/reactivegroup/sigs/issues/21

### 1. [技术分享](./sig/reactive/talk)

+ [Reactor-Core源码学习和使用](https://github.com/reactivegroup/sigs/issues/19)
+ [Reactor-Netty源码学习和使用](https://github.com/reactivegroup/sigs/issues/20)

[技术文章目录](./sig/reactive/talk/blog)

### 2. [技术项目](./sig/reactive/project)

+ [反应式的消息发送平台](https://github.com/reactivegroup/sigs/issues/12)
+ [Capa-Proxy](https://github.com/reactivegroup/sigs/issues/11)

### 3. [其他事务](./sig/reactive/affairs)

+ [迁移技术文章到github](https://github.com/reactivegroup/sigs/issues/9)

---

## RPC SIG

小组信息同步：https://github.com/reactivegroup/sigs/issues/24

### 1. [技术分享](./sig/rpc/talk)

+ [Dubbo&Netty源码学习](https://github.com/reactivegroup/sigs/issues/22)
+ [gRPC源码学习](https://github.com/reactivegroup/sigs/issues/23)

[技术文章目录](./sig/rpc/talk/blog)

### 2. [技术项目](./sig/rpc/project)

+ [参与Dubbo社区建设](https://github.com/reactivegroup/sigs/issues/15)
+ [Dubbo支持gRPC协议](https://github.com/reactivegroup/sigs/issues/25)
+ [SOA支持capa编程模式](https://github.com/reactivegroup/sigs/issues/18)

### 3. [其他事务](./sig/rpc/affairs)

+ [迁移技术文章到github](https://github.com/reactivegroup/sigs/issues/14)

---

## Cloud SIG

小组信息同步：https://github.com/reactivegroup/sigs/issues/28

### 1. [技术分享](./sig/cloud/talk)

+ [Istio源码学习](https://github.com/reactivegroup/sigs/issues/27)

[技术文章目录](./sig/cloud/talk/blog)

### 2. [技术项目](./sig/cloud/project)

+ [capa](https://github.com/reactivegroup/capa)
+ [layotto](https://github.com/reactivegroup/sigs/issues/6)

### 3. [其他事务](./sig/cloud/affairs)

+ [迁移技术文章到github](https://github.com/reactivegroup/sigs/issues/7)
