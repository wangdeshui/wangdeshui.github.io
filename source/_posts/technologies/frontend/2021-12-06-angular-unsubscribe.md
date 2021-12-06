---
layout: post
category : 技术
title: Angular实践系列(一)：取消(Unsubscribe )订阅 
date: 2021-12-06 20:00:00
tags: [FrontEnd]
---

Angular 用了大量的 Observable。我们经常开发的时候订阅后忘了取消订阅，这容易引起内存泄露，这里介绍两种取消Rxjs订阅的方法。

# Async Pipe
| async 异步管道使您可以在 HTML 模板中处理 Observable。异步管道在组件销毁过程后自动运行取消订阅过程。

```javascript
@Component({
    selector: 'cool-component',
    template: `
        <ul>
            <li *ngFor="let item of todoList$ | async">{{item.name}}</li>
        </ul>
    `
    ...
})
export class CoolComponent implements OnInit {
    private todoList$: Observable<string>;
    constructor(private httpService: HttpClient) {}
    ngOninit(): void {
        this.todoList$ = this.httpService.get('other-url.com')
    }
}
```



# takeUntil
takeUntil 可以在订阅之前在 .pipe() 方法中调用。 使用此方法，您可以将订阅添加到主题。 如果你有几个订阅，你可以在 ngOnDestroy 事件中使用 .next() 和 .complete() 方法取消订阅

假设您通过 HttpClient 发出多个 AJAX 请求。 您不会将其直接传递给 HTML，而是首先对数据执行其他操作。 所以| 异步管道不适合这种情况。现在您有多个订阅！我们怎样才能一次全部退订而不是一个一个退订呢？



```javascript
@Component({...})
export class CoolComponent implements OnInit, OnDestroy {
    private unsubscribe$ = new Subject<void>;
    constructor(private httpService: HttpClient) {}
    ngOninit(): void {
        this.httpService.get('some-url.com')
                .pipe(takeUntil(this.unsubscribe$))
                .subscribe((values) => {
                    // Do something with the data
                })
        
        this.httpService.get('other-url.com')
                .pipe(takeUntil(this.unsubscribe$))
                .subscribe((values) => {
                    // Do something with the data
                })
    }
    ngOnDestroy(): void {
        this.unsubscribe$.next();
        this.unsubscribe$.complete();
    }
}
```

在 get() 方法之后有一个 pipe(takeUntil(this.unsubscribe$))。 通过 takeUntil，我们将这个 Observable 的引用添加到 unsubscribe$ 主题。在订阅过程中，Subject 持有对两个 Observable 的引用。

在要销毁组件之前调用 ngOnDestroy() 方法, 在这个方法中，我们调用了两个方法,next() 将向订阅传递一个空值, 使用 complete()，我们告诉订阅它已完成侦听新值。现在我们不必担心通过 HttpClient 发出一个或多个请求； 我们可以立即阻止他们。





