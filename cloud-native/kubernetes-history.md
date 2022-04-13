# Kubernetes 的诞生

众所周知，[Kubernetes](http://kubernetes.io) 是 Google 于 2014 年 6 月基于其内部使用的 [Borg](https://research.google.com/pubs/pub43438.html) 系统开源出来的容器编排调度引擎。其实从 2000 年开始，Google 就开始基于容器研发三个容器管理系统，分别是 Borg、Omega 和 Kubernetes。这篇由 Google 工程师 Brendan Burns、Brian Grant、David Oppenheimer、Eric Brewer 和 John Wilkes 几人在 2016 年发表的[《Borg, Omega, and Kubernetes》](https://static.googleusercontent.com/media/research.google.com/en/pubs/archive/44843.pdf)论文里，阐述了 Google 从 Borg 到 Kubernetes 这个旅程中所获得知识和经验教训。

## Borg、Omega 和 Kubernetes

Google 从 2000 年初就开始使用容器（Linux 容器）系统，Google 开发出来的第一个统一的容器管理系统在内部称之为 “Borg”，用来管理长时间运行的生产服务和批处理服务。由于 Borg 的规模、功能的广泛性和超高的稳定性，一直到现在 Borg 在 Google 内部依然是主要的容器管理系统。

Omega 是 Borg 的后代，是为了改进 Borg 生态系统的软件工程。它使用了许多在 Borg 中被证明是成功的模式，但它是独立开发的，具有更一致、更有原则的架构。Omega 将集群的状态存储在一个集中的、基于 Paxos 的、面向事务的存储中，由集群[控制平面](kubernetes-history.md#can-kao)的不同部分（例如调度程序）访问，使用[乐观并发控制](kubernetes-history.md#can-kao)来处理偶尔的冲突。这种分离使得 Borg master 的功能可以被分解成单独的组件，作为对等组件，而不是通过一个单一的、集中的主控器来实现每一个更改。此后，Omega 的许多创新（包括多个调度程序）都被并入了 Borg。

Google 的第三套容器管理系统就是我们所熟知的 Kubernetes，它的诞生有两个原因，拓展公有云底层业务，以及吸收对 Linux 容器技术感兴趣的外部开发者。Kubernetes 是开源的，跟 Omega 一样，Kubernetes 的核心是一个共享的持久化存储，有组件来检测相关 object 的变化。跟 Omega 不同的是，Omega 把存储直接暴露给信任的控制平面的组件，而在 Kubernetes 中的状态仅通过特定于域的 REST API 访问，该 API 应用更高级别的版本控制、验证、语义和策略，以支持更多样化客户群。更重要的是，Kubernetes 是由一群底层开发能力更强的开发者开发的，他们主要的设计目标是用更容易的方法去部署和管理复杂的分布式系统，同时仍能从容器提升的效率中受益。

2014 年 Kubernetes 正式开源，2015 年被作为初创项目贡献给了云原生计算基金会（CNCF），从此开启了 Kubernetes 及云原生化的大潮。



## 参考

* [Borg, Omega, and Kubernetes: Lessons learned from three container-management systems over a decade - queue.acm.org](https://queue.acm.org/detail.cfm?id=2898444)
* [Borg、Omega 和 Kubernetes：谷歌十几年来从这三个容器管理系统中得到的经验教训 - dockone.io](http://dockone.io/article/1153)
* 乐观并发控制：它假设在处理多用户并发时不会彼此互相影响，各事务能够在不产生锁的情况下处理各自影响的那部分数据。
* [集群控制平面：容器编排层，公开用于定义、部署和管理容器生命周期的API和接口。](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane)
