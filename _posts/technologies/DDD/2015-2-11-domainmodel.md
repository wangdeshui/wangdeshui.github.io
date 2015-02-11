---
layout: post
category : 技术
title: 领域驱动设计系列（二）:领域Model？
date: 2015-02-11 7:00:00
tags: [DDD]
---

## 前言
领域驱动设计里有很多东西，我们可以应用在各种各样的开发模式里，所以接下来说的一些东西，我们可以部分使用。

说道领域驱动的领域，大家肯定就要开始说Bounded Context,聚合，聚合根，容易让大家搞糊涂。 我觉得先抛开这些概念，后面再来说如何设计聚合，先简单来说。

## 模型

过去，我们在多层设计里定义了很多Model, 数据库的Model(DB Entity), 然后为了不依赖数据库，我们有设计了业务的Domain Model, 同时我们又设计了ViewModel, 这样一般也没什么问题，职责也很清晰。但是有几个问题

1. 我们要做很多的模型转换，转入转出。当然我们可以用AutoMapper来但是AutoMapper的性能实在难以恭维，大家可以在网上搜索AutoMapper performance.
2. 领域模型成了一个单纯的DTO了。

## 领域模型

首先我们要看领域，就是我们尽量把业务聚合到一个领域里，比如我们要做一个功能，可以看到用户每一次的登录日志，那个这个登录日志其实就是属于用户这个领域里。

其次我们看模型，原来我们的模型都是只有属性，也就是贫血模型，贫血的意思就是没有行为，像木乃伊一样，但是实际上领域是我们要完成业务的最主要的地方，我们希望领域能够自制，也就是领域自己管理自己。

### 示例

比如有一个Employee, 他的状态有Active, Pending, DeActive, 业务上是Pending只能改为Active. 

    {% highlight C# %}
	public class Employee : Entity
    {
        public Name Name { get; set; }
       
        public EmployeeStatus EmployeeStatus { get; set; }

    }
    {% endhighlight %}
	
如果是贫血的Employee模型，我们往往代码如下


	{% highlight C# %}
	public class EmployeeService : IEmployeeService
    {
        private readonly IUnitOfWorkFactory _unitOfWorkFactory;
        private readonly IEmployeeRepository _employeeRepository;

        public EmployeeService(IUnitOfWorkFactory unitOfWorkFactory, IEmployeeRepository employeeRepository)
        {
            _unitOfWorkFactory = unitOfWorkFactory;
            _employeeRepository = employeeRepository;
        }

        public void ChangeStatus(EmployeeStatus status, Guid employeeId)
        {
            using (var unitOfWork = _unitOfWorkFactory.GetCurrentUnitOfWork())
            {
                var employee = _employeeRepository.GetById(employeeId);
                employee.EmployeeStatus = status;

                unitOfWork.Commit();
            }
        }
    }
	{% endhighlight %}


但是上面的代码的问题就是领域没有自治，本来修改我的状态是我的事，你能不能修改，外面随意修改我的状态是很危险的，比如Pending状态只能改为Active状态。 所以如果不是贫血的模型，我们代码就会这样，让领域自己来管理

	
	{% highlight C# %}
	public class Employee : Entity
    {
   
        public UserId UserId { get; private set; }
        public EmployeeStatus EmployeeStatus { get; private set; }


        public void ChangeStatus(EmployeeStatus status)
        {
            if (this.EmployeeStatus == EmployeeStatus.Pending && status != EmployeeStatus.Active)
            {
                throw new Exception("Only can Active when status is pending");
            }

            this.EmployeeStatus = status;
        }

    }

	public class EmployeeService : IEmployeeService
    {
        private readonly IUnitOfWorkFactory _unitOfWorkFactory;
        private readonly IEmployeeRepository _employeeRepository;

        public EmployeeService(IUnitOfWorkFactory unitOfWorkFactory, IEmployeeRepository employeeRepository)
        {
            _unitOfWorkFactory = unitOfWorkFactory;
            _employeeRepository = employeeRepository;
        }

        public void ChangeStatus(EmployeeStatus status, Guid employeeId)
        {
            using (var unitOfWork = _unitOfWorkFactory.GetCurrentUnitOfWork())
            {
                var employee = _employeeRepository.GetById(employeeId);
                employee.ChangeStatus(status);

                unitOfWork.Commit();
            }
        }
    }
    {% endhighlight %}
	

因此可以看出，我们把业务代码尽量写在领域里让领域自治。 

## 后记
其实领域驱动设计最难的就是设计领域(Domain), 也就是后面会说到的AggregateRoot 聚合，但是我想我们先让领域不再贫血，这样在传统的多层设计，数据驱动等架构都可以使用这种模式。

