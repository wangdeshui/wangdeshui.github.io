---
layout: post
category : 技术
title: 领域驱动设计(四)：事件驱动下
date: 2015-02-11 15:00:00
tags: [DDD]
---


# 前言
上一篇说到为什么要使用事件驱动，但是只有概念是不够的，我们要代码呀！记得脸书的老总说过: "Talk is cheap, Show me the code!"

# 实现思路

## 发出事件

事件顾名思义就是一件事情发生了，比如我要上头条，这不是一个事件，这事一个Command, HeadCommand, 而我上头条了这就是一个事件HeadedEvent，事件就是一件事情已经发生了。 好，先来一个伪代码

       public void Head()
        {
            var NewsPaper = new NewsPaper("南都娱乐");
            NewsPaper.WriteToHeader("汪峰");

            RaiseEvent(new HeadedEvent {Name = "汪峰"});
        }

所以我们只需在代码里RaiseEvent就可以了。


## 那么如何订阅事件

其实很简单，因为我们要实现的是同步的事件，我们只需要找到所有处理这个事件的实现类，然后调用所有就可以了。 

    public interface IEventHandler<TEvent> where TEvent : Event
    {
        void Handle(TEvent e);
    }

    public class HeadedEvent:Event
    {
        public string Name { get; set; }
    }


如果国际章的妈妈关注这个Event, 我们就实现一个GuoJiZhangMotherEventHandler

    public class GuoJiZhangMotherEventHandler : IEventHandler<HeadedEvent>
	{
	    public void Handle(HeadedEvent e)
	    {
	         Console.WriteLine(e.Name+", Are you kidding me?");
	    }
	}

如果我等屁民也关心这个事件的话，我们只需要再实现一个 PiMingEventHandler

    public class PiMingEventHandler:IEventHandler<HeadedEvent>
	{
	    public void Handle(HeadedEvent e)
	    {
	        Console.WriteLine(e.Name+", Guo Ji Zhang is your last wife?");
	    }
	}
   

   
看，我们可以任意增加关注事件的代码，不用修改原来的代码吧，说好的OCP没骗你吧？ 那么问题来了，发出事件的人和接受事件的人怎么联系上的？在现实世界中，我们都是订阅报纸来看头条知道的，但是代码里我们就需要一个协调者了。如是我们就需要一个EventBus, 直接上代码吧

        public void Head()
        {
            var NewsPaper = new NewsPaper("南都娱乐");
            NewsPaper.WriteToHeader("汪峰");

            RaiseEvent(new HeadedEvent {Name = "汪峰"});
        }

        private void RaiseEvent(HeadedEvent headedEvent)
        {
            EventBus.Publish<HeadedEvent>(new HeadedEvent { Name = "汪峰" });
        }


 EventBus找出所有Handle这个事件的实现类，调用对应的Handle方法，我们可以通过Castle或者任何注入框架轻易的实现
    

	public class EventBus
    {
        public static void Publish<T>(T concreteEvent) where T: Event
        {
            var handlers = _container.ResolveAll<IEventHandler<T>>();
            foreach (var handle in handlers)
            {
                handle.Handle(concreteEvent);
            }
        }
    }

好了，哥只负责帮汪老师上头条，上完我发出了事件通知，谁关注谁自己处理去，我的代码也不用改。

我代码实现完了，如果各位还不知道如何实现一个同步的事件驱动架构，那拜托你们找个漂亮的妹子来问我。事件驱动架构我就只能帮你到这里了。