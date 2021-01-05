---
layout: post
category : 技术
title: 领域驱动设计系列（三）:事件驱动上
date: 2015-02-11 10:00:00
tags: [DDD]
---


# 前言

今天讲一下事件驱动，这个不是领域驱动设计里的事件源(Event Source), 这个以后再讲，今天主要讲一下如何用事件来解耦，主要的原因是我们有个项目有个功能我觉得用事件的方式比较好，正好写篇博客，就不用专门给他们讲了。

# 解耦

说到解耦，我们很熟悉分层设计，比如上层依赖于抽象，不依赖于具体的实现。比如一个类使用另一个类，我们使用接口而不直接使用实现类。

     public EquipmentService(IEmailService emailService, IEquipmentRepository equipmentRepository)
        {
            _emailService = emailService;
            _equipmentRepository = equipmentRepository;
        }



# 为何用事件？

## SRP （单一职责)

比如我们一个会议室预定系统，我们的一个设备坏了。我们需要通知预定这个会议室的所有人。于是我们需要发邮件。

伪代码如下

    public class EquipmentService
    {
        private readonly IEmailService _emailService;
        private readonly IEquipmentRepository _equipmentRepository;

        public EquipmentService(IEmailService emailService, IEquipmentRepository equipmentRepository)
        {
            _emailService = emailService;
            _equipmentRepository = equipmentRepository;
        }

        public void SetEquipmentBroken(string Id)
        {
            var equipment = _equipmentRepository.GetById(Id);
            equipment.DeActive();

            _emailService.SendEmail();
        }
    }


但是，问题来了，如果后来我们要说，如果设备坏了，我们要更改可用库存的数量，这时候我们是不是要在这里修改代码而引入IInventoryService? 后来如果经理说设备坏了你们尽然不告诉我，你们要闹哪样？这个时候我们是不是要修改代码引入ISMSService.Info(Manager)? 即使我们不考虑OCP原则，不考虑单一职责，我们程序员也会哭，我就DeActive一个设备，你要我做这么多事，我哪里清楚所有的功能？我就骂过程序员，你做这个功能呢为什么没考虑全！！！漏掉了这么重要的功能。

而问题，程序员从来没考虑全过，因此我就想办法如何解决这个程序员不仔细的问题。

## 事件驱动

因为我熟悉iOS的开发，我就想到了iOS的Notification Center. 那我我DeActive一个设备，我就只DeActive这个设备，很SRP是不是？ 但是别的地方如何拿到通知？ 于是事件就自然的付出水面了。如果设备被DeActive了，程序就只需要喊一声，老子把设备DeActive了，你们要闹哪样你们自己看着办，代码如下。

     public void SetEquipmentBroken(string Id)
        {
            var equipment = _equipmentRepository.GetById(Id);
            equipment.DeActive();

            EventBus.Publish(new EquipmentDeActivedEvent {Id = equipment.Id});

        }

这样，通知会议室预定者的模块去通知，给老板发短信的模块就通知老板就OK了。


## 总结
这里我们先将事件驱动，下一篇展示如何实现同步的事件，以后转换为异步那也很容易，让多个接受者处理这个事件，接受者可以是动态的哦，以后老板娘也想知道的话，代码也不用改的亲，好，我先去写实现代码去！