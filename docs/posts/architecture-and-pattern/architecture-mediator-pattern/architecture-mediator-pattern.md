---
title: Mediator 模式及其 .NET 实现
description: 本文介绍了「中间人」模式及其应用场景，以及 .NET MediatR 类库如何实现「中间人」模式
excerpt: 本文介绍了「中间人」模式及其应用场景，以及 .NET MediatR 类库如何实现「中间人」模式
author: Frost He
date: 2018-02-28
lang: zh-CN
category:
- Architecture and Pattern
tag:
- architecture
- mediator-pattern
- dotnet
---

参考资料:
- [Mediator pattern](https://en.wikipedia.org/wiki/Mediator_pattern)
- [Simplifying Development and Separating Concerns with MediatR](https://blogs.msdn.microsoft.com/cdndevs/2016/01/26/simplifying-development-and-separating-concerns-with-mediatr/)
- [MediatR](https://github.com/jbogard/MediatR/wiki)

## 中间人模式简介
在软件工程层面，中间人模式定义了**封装不同对象之间的交互方式**。在应用系统中，随着业务体量的逐渐增大，不同对象之间的通信变得越来越复杂，这使得应用程序难以阅读及维护。中间人模式将对象之间的「通信(或交互)」封装至单独的「中间人(Mediator)」对象中，对象之间通过「中间人」进行通信，而不再直接交互。**这减少了拟交互对象之间的依赖，进而降低了系统内部各组件之间的耦合**。

## 定义
中间人模式的要点，在于封装**拟交互对象群间关系**，拟交互类型通过「中间人」将消息发送至其他拟交互类型，并同时从其他拟交互类型接收消息。

## 案例
在 MVC 项目中，应用层的 `Controller` 通常以 DI 的方式依赖其他组件，例如，我们经常看到如下的 `Controller`:
```csharp
public DashboardController(
    ICustomerRepository customerRepository
    )
```
一开始，这种实现并无不妥，但随着业务规则越来越复杂，它很快便会成长为:
```csharp
public DashboardController(
    ICustomerRepository customerRepository,
    IOrderService orderService,
    ICustomerHistoryRepository historyRepository,
    IOrderRepository orderRepository,
    IProductRespoitory productRespoitory,
    IRelatedProductsRepository relatedProductsRepository,
    ISupportService supportService,
    ILog logger
    )  
```
也许有人看到这段代码会觉得 `Controller` 承担了太多职责，于是开始思考应该将这些职责转移到更适合的下层组件当中去。然而，无论这些依赖存在于哪个组件都显得太臃肿了，因为它显式依赖了这些对象(无论是具体类型还是抽象类型)。

为了解决这个问题，我们引入中间人模式，让所有依赖其他对象进行交互的类型转而引用中间人类型，使得耦合度大大降低:
```csharp
public DashboardController(
    IMediator mediator
    )
```
签名看上去清爽多了，但实际的通信和交互应该如何实现呢？让我们看看如何将一个简单的命令通过 MediatR 应用在真实的项目中。

## MediatR 类库
[MediatR](https://github.com/jbogard/MediatR) 是 .NET 生态中实现「中间人」模式的轻量级类库，支持 `Request`/`Notification` 两种通信方式，在编码实现上支持同步/异步编程模型，我们以一个 ASP.NET Core 项目为例来演示如何实现这一过程，关键步骤为:
1. 通过 Nuget 添加对 `MediatR` 的项目引用
2. 创建 `Request/Notification` 对象
3. 创建处理 `Request/Notification` 的 `Handler` 对象

### 在应用层中注册 IMediator 和 ServiceFactory 委托
按照 [MediatR Wiki](https://github.com/jbogard/MediatR/wiki)， `IMediator` 使用 `ServiceFactory` 委托创建所需的 `Handler` 类型实例、`Pipiline` 行为和 `pre/post` 处理器。它们的服务类型及实现类型需要在应用层中进行注册，添加 `Autofac.Extensions.DependencyInjection` 库的引用:
```csharp
        public IServiceProvider ConfigureServices(IServiceCollection services)
        {
            services.AddAutofac();
            var builder = new ContainerBuilder();
            builder.Populate(services);
            builder.RegisterAssemblyTypes(typeof(IMediator).Assembly).AsImplementedInterfaces();
            builder.RegisterAssemblyTypes(typeof(Startup).Assembly).AsImplementedInterfaces();
            builder.Register<ServiceFactory>(ctx =>
            {
                var context = ctx.Resolve<IComponentContext>();
                return type => context.Resolve(type);
            });

            var container = builder.Build();
            return new AutofacServiceProvider(container);
        }
```
上述代码的关键两行为
- `builder.RegisterAssemblyTypes(typeof(IMediator).Assembly).AsImplementedInterfaces()`: 将 `IMediator` 接口的默认实现以单例模型注册至服务容器
- `builder.Register<ServiceFactory>(ctx =>`: 定义 `ServiceFactory` 委托的实现，该委托旨在提供**在运行时解析类型的实现**

### 创建实现 IRequest/INotification 接口的类型
接下来，创建一个实现了 `IRequest` 接口的类型，该类型代表某个希望完成的工作:
```csharp
public class AddEmergencyContactCommand : IRequest
{
    public string Contact { get; set; }
    public string UserId { get; set; }
}
```
该类型仅包含一些 `payload` 属性，理论上可以在 `Request` 对象中定义任何需要发送至 `Mediator` 的载荷数据——基元类型或复杂类型都支持。

### 创建处理对应 Request/Notification 的 Handler 的类型
`Handler` 类型继承自 `RequestHandler<TRequest>` 类型，并重写了 `Handle` 方法。`Mediator` 将确保 `Handle` 方法传入期望的 `Request` 对象。
```csharp
public class AddEmergencyContactHandler : RequestHandler<AddEmergencyContactCommand>
{
    protected override void Handle(AddEmergencyContactCommand request)
    {
        Console.WriteLine($"Emergency contact added: {request.Contact} by {request.UserId}.");
    }
}
```

`Handler` 类型可通过 DI 访问其他类型的引用以确保完成命令操作。`Command` 的发起者(本例中为 `Controller`)将不再关心执行该命令要依赖哪些对象，仅需知道要发起何种命令:
```csharp
        [HttpPost]
        public IActionResult AddEmergencyContact(string userId, string contact)
        {
            if (ModelState.IsValid)
            {
                var command = new AddEmergencyContactCommand { Contact = contact, UserId = userId };
                _mediator.Send(command);
                return Ok();
            }
            return BadRequest();
        }
```
`Controller` 的 `Post` 方法中，组装了 `AddEmergencyContactCommand` 对象，并通过调用 `Mediator.Send()` 方法发送命令。`Mediator` 对象将确保调用栈的下一步指向对应的 `AddEmergencyContactHandler` 类型并将 `AddEmergencyContactCommand` 作为参数传入其 `Handle` 方法。类型的解析委托正是前文在 `Startup.ConfigureServices` 方法中定义的 `ServiceFactory`。

### 使用一对多的处理方式 - Notification
`Request` 模拟了一对一的靶向操作，而 `Notification` 则模拟了一对多的发布/订阅操作。与 `Request` 类似，首先需要定义通知消息:
```csharp
    public class ContactAddedNotification : INotification
    {
        public string Contact { get; set; }
    }
```
接下来，定义两个消费该通知消息的 `Handler` 类型:
```csharp
    public class ContactAddedPong1 : INotificationHandler<ContactAddedNotification>
    {
        public Task Handle(ContactAddedNotification notification, CancellationToken cancellationToken)
        {
            Debug.WriteLine($"{nameof(ContactAddedPong1)} Received {nameof(ContactAddedNotification)}, contact: {notification.Contact}.");
            return Task.CompletedTask;
        }
    }

    public class ContactAddedPong2 : INotificationHandler<ContactAddedNotification>
    {
        public Task Handle(ContactAddedNotification notification, CancellationToken cancellationToken)
        {
            Debug.WriteLine($"{nameof(ContactAddedPong2)} Received {nameof(ContactAddedNotification)}, contact: {notification.Contact}.");
            return Task.CompletedTask;
        }
    }
```
基于前文的 `Request` 的案例，发布通知，修改代码如下:
```csharp
    public class AddEmergencyContactHandler : AsyncRequestHandler<AddEmergencyContactCommand>
    {
        private readonly IMediator _mediator;
        public AddEmergencyContactHandler(IMediator mediator)
        {
            this._mediator = mediator;
        }

        protected async override Task Handle(AddEmergencyContactCommand request, CancellationToken cancellationToken)
        {
            Debug.WriteLine($"Emergency contact added: {request.Contact} by {request.UserId}.");

            var notification = new ContactAddedNotification
            {
                Contact = request.Contact
            };

            await this._mediator.Publish<ContactAddedNotification>(notification);
        }
    }
```
`IMediator` 使用 `Publish` 方法来发布通知。上述代码同时将原来继承的 `RequestHandler` 替换成了 `AsyncRequestHandler` 以支持异步编程模型。

> 需要指出的是，`Mediator` 只是在**编码级别**做了类似「消息队列」的工作，实际的调用序列依然是同步的，`Mediator` 关注的重点在于「解耦」，而非「异步」。

本 Sample 的源代码可至 [Github](https://github.com/BerdyPango/tastes-of-libraries) 下载。