---
title: Repository 模式和工作单元
description: 本文介绍了 Repository 模式及其应用场景，以及在 .NET 中是如何实现 Repository 模式的
excerpt: 本文介绍了 Repository 模式及其应用场景，以及在 .NET 中是如何实现 Repository 模式的
author: Frost He
date: 2017-03-18
lang: zh-CN
category:
- Architecture and Pattern
tag:
- architecture
- repository-pattern
- dotnet
---

参考资料:
- [Repository Pattern](https://martinfowler.com/eaaCatalog/repository.html)
- [UnitOfWork](https://martinfowler.com/eaaCatalog/unitOfWork.html)
- [Design the infrastructure persistence layer](https://docs.microsoft.com/en-us/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)

## Repository 模式
`Repository` 是引入域模型与数据源提供器之间的对象，它们封装了访问数据源的功能，抽象出通用的数据访问方法，提供了更好的可维护性并将域模型与具体的数据访问组件进行解耦。

在「企业应用架构模式」一书中，**Martin Fowler** 这样描述一个 Repository:
> *A repository performs the tasks of an intermediary between the domain model layers and data mapping, acting in a similar way to a set of domain objects in memory. Client objects declaratively build queries and send them to the repositories for answers. Conceptually, a repository encapsulates a set of objects stored in the database and operations that can be performed on them, providing a way that is closer to the persistence layer. Repositories, also, support the purpose of separating, clearly and in one direction, the dependency between the work domain and the data allocation or mapping.*

## Repository 模式让数据提供层便于测试
`Repository` 模式使得编写单元测试代码变得更加简单，当 `Repository` 模式的接口位于独立于具体实现的工程，便可采用模拟数据源的方式编写「假的」`Repository` 实现，让所有依赖 `Repository` 的类型的测试不再依赖具体的数据源实现，而将测试的重点放到域模型的核心逻辑上。

## 工作单元
当将数据读取或写入数据库时，跟踪修改记录是很重要的一环，否则将无法回滚数据。在实践中，可以一旦对象状态发生变化，就修改数据库中对应的数据，这种方式将导致非常多的小的数据库访问，速度将非常慢；也可以在对象交互期间开启事务，但这会引起并发任务时的数据不同步问题。「工作单元」跟踪对对象所做的所有变更，并最后反映在数据库中。

## 传统 DAL 层与 Repository 模式的区别
数据访问对象直接在存储引擎上执行数据访问逻辑，而Repository首先在内存中标记对象的状态变化，进而在恰当的时机提交事务。`Repository` 采用「工作单元」来处理事务。这意味着一个具体的用户的行为(例如注册账号)造成的系统数据变化将在一个事务中处理。

使用 `Repository` 隐含着一种意图倾向，即域模型需要什么 Repository 便提供什么，一切都以域的需求为核心。而 DAL 则是作为组件存在，它不关心业务规则。