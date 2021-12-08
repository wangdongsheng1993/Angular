# RxJS （Reactive Extensions for JavaScript）

#### 用来做什么？
 处理异步回调
#### 和Promise的区别？
 RxJS可以取消订阅，Promise不可以
 RxJS可以发布多次，Promise只有一次就freeze
 RxJS集成Promise,回调，事件数据 例如DOM Input，Web Worker, Web Socket
#### 特点
Observables + Operators + Schedulers （可观察对象+运算符+调度器）
用Observable表示异步数据流
用Operator查询异步数据流
用Scheduler使异步数据流的并发性参数化
#### 兼容
支持IE6+、Chrome4+、Firefox1+
支持Node.jsV0.4+


## 基本概念
Pull  Pull是由消费者决定何时获取数据
Push  Push是由生产者决定何时发送数据
Promise和RxJS引入的Observable都是JavaScript的Push系统
#### Observable (可观察对象)
Observables是多个值的延迟推送集合
      SINGLE	MULTIPLE
Pull	Function	Iterator
Push	Promise	Observable
    
#### Observer (观察者)
Observer是Observable传递给消费者的值
Observers是一组回调，由Observable提供的每种类型的通知都有一个回调：next、error和complete

> observable.subscribe(observer);

#### Subscription (订阅)
表示 Observable 的执行，主要用于取消 Observable 的执行。
Operators (操作符): 采用函数式编程风格的纯函数 (pure function)，使用像 map、filter、concat、flatMap 等这样的操作符来处理集合。
Subject (主体): 相当于 EventEmitter，并且是将值或事件多路推送给多个 Observer 的唯一方式。
Schedulers (调度器): 用来控制并发并且是中央集权的调度员，允许我们在发生计算时进行协调，例如 setTimeout 或 requestAnimationFrame 或其他

