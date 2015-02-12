---
layout: post
category : 技术
title: 领域驱动设计系列（五）:事件驱动之异步事件
date: 2015-02-12 16:00:00
tags: [DDD]
---


# 前言

上一篇讲了事件，以及为什么要使用事件，主要是为了解耦，但是有同学就问了，同步如果订阅事件的人太多，比如13亿人都关心上头条的事，那么RaiseEvent得等13亿人都处理完，那得多久呀，从此再也不敢发事件了。
举个例子，你在网上下单，下完单要通知库房，甚至要通知供应商补货，如果都是同步的话，消费者还不等急死呀，实际上你在电商网站上下个单， 一般你很快就能到订单页面，那个页面告诉你：“兄弟，订单已经创建成功，订单号是xxxxx-xxxxx-xxxx-xxxx,你的订单已经提交到库房” 等。然后你就很快了的下另一单了。好吧，
提问的同学，说好的妹子呢？

# 实现思路

## 发出事件

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

所以我们只需在代码里RaiseEvent就可以了。


## 订阅事件

其实很简单，因为我们要实现的是同步的事件，我们只需要找到所有处理这个事件的实现类，然后调用所有就可以了。 

    public interface IEventHandler<TEvent> where TEvent : Event
    {
        void Handle(TEvent e);
    }

    public class HeadedEvent:Event
    {
        public string Name { get; set; }
    }

    public class GuoJiZhangMotherEventHandler : IEventHandler<HeadedEvent>
	{
	    public void Handle(HeadedEvent e)
	    {
	         Console.WriteLine(e.Name+", Are you kidding me?");
	    }
	}

    public class PiMingEventHandler:IEventHandler<HeadedEvent>
	{
	    public void Handle(HeadedEvent e)
	    {
	        Console.WriteLine(e.Name+", Guo Ji Zhang is your last wife?");
	    }
	}
   

   
我们可以看到正真的事件协调者是EventBus, 之前的代码如下是同步的。


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

为了提高性能，我们可以先来第一步改进
    

     public void Publish<T>(T @event) where T : Event
     {
        var handlers = _eventHandlerFactory.GetHandlers<T>();

        handlers.AsParallel().ForAll((h)=> h.Handle(@event));
       
     }

我们可以看到，现在并行处理可以大大加快速度，但是有两个问题，第一个问题就是没有处理异常，所以让我们加上异常。

     public void Publish<T>(T @event) where T : Event
        {
            var handlers = _eventHandlerFactory.GetHandlers<T>();

            handlers.AsParallel().ForAll((h)=> HandleEvent<T>(h,@event));
           
        }

        private void HandleEvent<T>(IEventHandler<T> handle, T @event) where T : Event
        {
            try
            {
                handle.Handle(@event);

            }
            catch (Exception e)
            {
                
               // Log the exception, as the caller don't care this
            }
        }
    }

第二个问题，就是我们虽然用了并行加快了速度，但是还没有正真实现异步，整个程序还是等所有Handler处理完才返回。

       public void Publish<T>(T @event) where T : Event
        {
            var handlers = _eventHandlerFactory.GetHandlers<T>();

            handlers.Select(h => Task.Factory.StartNew(() => HandleEvent<T>(h, @event)));
           
        }

这段代码执行完，尽然发现Handler没有执行，好吧，原因是IQueryable的延迟执行，所以我们需要调用一下ToList

    public void Publish<T>(T @event) where T : Event
        {
            var handlers = _eventHandlerFactory.GetHandlers<T>();

            handlers.Select(h => Task.Factory.StartNew(() => HandleEvent<T>(h, @event))).ToArray();
           
        }

好了，我们就这样轻易的实现了一个AsyncEventBus, 是不是感谢.Net的强大?


## 总结

这里还只是一个系统内部的Async, 如果涉及到系统之间的交互，这个就不行了，而且如果异步处理有错误，我们就会有信息丢失，所以需要更健壮的异步事件处理系统，这个后面再讲，但是一般的系统我们只需要把出错的时间记录下来，然后再看要不要处理就可以。

另外，异步要处理的东西很多，比如处理完毕后，如何通知用户，还是让用户刷新？ 我个人建议，一般情况下都不要用异步，只有在真的需要的时候再用。