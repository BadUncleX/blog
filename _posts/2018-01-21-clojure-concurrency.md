---
layout: post
title:  " Clojure并发 "
date:   2018-01-21 10:08:15 +0800
keywords: #clj #clojure #concurrency #并发 #多线程
categories: clojure concurrency
math: true
typora-copy-images-to: ../assets/images/2018-01
typora-root-url: ../../blog_alexcode
---
<h2>Table of contents</h2>
* TOC
{:toc}
## concurrency和 parallel区别

> from CSAPP

并发（Concurrency）是说进程B的开始时间是在进程A的开始时间与结束时间之间，我们就说A和B是并发的。
并行（Parallel Execution）是并发的真子集，指同一时间两个进程运行在不同的机器上或者同一个机器不同的核心上。

>  并发： 一个CPU交叉执行任务时典型的并发， 关键词“假象”
>
> 并行： 多核心执行的多任务，关键词“真爷们”



银行里的柜台一个工作人员服务一位顾客，这是串行， 如果A顾客在填写表格的同时服务B顾客， 这可以叫并发， 即有“交叉”执行这个动作。  并发更多的是提供“花样”让你感觉快， 但未必有多快。 



有两个柜台， 这就可以有并行的能力了， 即同时服务2个顾客， 比前面的一个工作人员服务两个顾客还快。  并行是真正提供更多的计算能力。 





再举一个例子：

詹姆士邦德和上午和A美女做happy的事情， 下午和B美女做happy的事情， 这是串行， 一天最多和2位美女happy。 



现在邦德要提高效率， 两个美女同时进房间， “左拥右抱”， 在形式上邦德一次可以玩两个美女。 这是并发。 

这里有一个问题， 两个美女都不太爽， A美女刚high起来邦德又切换到B美女了， 搞得两个美女怨声载道。 



最后， 邦德邀请另一位猛男加入， 一人一个美女，这是并行， 两位美女都最大程度得到满足。 





如何提高并发(concurrency)能力?

将要做的事分解成独立的小单元， 让这些更小单元的事情可以同时做，最好相互之间没有依赖。 



如何提高并行(Parallel)能力?

增加更多的CPU。 



综合来看， 如果我们希望一个应用的性能很高， 光有很多的CPU运算资源还是不够的， 还需要应用自身支持并发能力， 即自身可以分割成没有依赖的小任务， 然后这些小任务可以同时在多个CPU上运行， 性能就上来了。 





As far as the JVM is concerned, you can have unlimited threads. And they're all able to read and write to the same memory. Obviously, all of those threads will need some help working together.

Let's get into the concurrency tools we've got in Clojure

> JVM提供足够多的线程， 每个线程都可以读写相同的内存， 我们需要其他的工具协作才能保证这个读写是安全有效的。 那么Clojure里提供了哪些工具呢？ 





## Clojure给线程协作提供的工具



### Immutable data structures

简单说，就是数据永远是简单的，不会被乱七八糟的人乱七八糟的修改得到乱七八糟的结果， 一句话， 数据永远不变，每次返回的是一个新的数据。 这个原则确保了我们的操作的简单， 因为没有意料之外的事情发生了， 你可以认为“数据总是干净的”。 





### Pure function

A pure function gives you the same return value for the same inputs all the time. 



哪些操作不是Pure function?

- 用户输入
- 修改全局变量
- fetch something from network
- 获取random number



和immutable data structure一样， pure function的意义是“简单，可信任，没意外”， 它很“忠诚”， 每次的行为都是一致的， 不存在“看情况”的情形。 



这是纯函数的优点一，即易于理解。 



纯函数的优点二： 没有副作用。  可以重试。  （运用于乐观锁）









### Independent

如果10个人用袋子背石头上山， 如何运最多的石头上山？  一般的算法设计“背包算法”， 如何提高呢？

将石头打碎， 这样背包不用考虑如何分配石头， 直接装满为止。 



类比到Clojure的cpu任务分配， 我们最佳的办法就是将任务分解的足够细，这样就不用考虑先执行哪个后执行哪个， 任务就可以在更多的cpu上运行。 





需要注意一个问题： 有些任务分解的太细可能有反作用， 即调度的开销太大， 反倒比原来慢了。 



有两类independent，

-  Pure functions are **independent of time**
- **order independence **?? 



order independence: 比如订单生成和结账两个动作， A和B两个顾客， A的订单生成比B早并不能保证A比B先结完账， 但是， 对于A来说， 一定是先生成订单， 后结账， 这是有顺序的。 





## delay

> A Delay ensures that some code is either run zero or one time. It's run zero times if the Delay is never `deref`ed. 



Delays are also useful for a shared resource between threads.

```clojure
;; make a delay that initializes a shared resource
;; if we don't put it in a Delay, the resource will
;; be initialized during compilation
(def resource (delay (initialize-shared-resource)))

;; start 100 threads that use the resource
(defn -main []
  (dotimes [x 100]
    (doto (Thread. (fn []
                     (let [resource @resource]
                       ;; use the resource here ...
                       ))
                   .start)))
```



## clojure model of state and identity

identity: 唯一标识，类比你的身份证编号

state：随着时间变化你的各个阶段的状态， 比如年龄， 重量。

上面的identity和state都是immutable的， 体现变化的是一个新的state。 



我们得到一个identity的current value, 然后operate， 然后需要更新的时候我们借助协作机制 (coordination),  (这个过程的最大特点是， 我们不用考虑变量的变化以及同步，因为都是immutable的)



当我们说coordination的时候， 我们指的是STM , ref. 

![2EF7D587-0DFD-46AD-8B5D-69B205CC2640](/assets/images/2018-01/2EF7D587-0DFD-46AD-8B5D-69B205CC2640.png)

## atom and swap!  CAS

> The Atom guarantees that anybody derefing the Atom always gets either the old value or the new value, and never anything in between.



看一个atom来解决捐款的例子：(一段时间后发tweet)

```clojure
;; create an Atom with initially no money
(def donation-count (atom 0))

;; start 9 people collecting money (9 threads)
(dotimes [_ 9]
  (doto (Thread. (fn []
                   ;; wait three seconds
                   (Thread/sleep 3000)
                   ;; go collect $1
                   (swap! donation-count inc)
                   ;; do it again
                   (recur)))
    .start))

;; start one person tweeting
(doto (Thread. (fn []
                 ;; wait 100 seconds
                 (Thread/sleep 100000)
                 (tweet (str "We collected $" @donation-count " total!"))))
  .start)

```

```clojure
(comment
    (def x (atom 1))
    (deref x) ;;=> 1
    @x ;;=> 1

    (swap! x inc) ;;=> 2
    @x ;;=> 2

    (swap! x (partial + 1))
    ;;=> 3

    ;; The reset! function sets the value
    ;; of atom regardless of the current
    ;; value of atom
    (reset! x 1)
    ;;=> 1)

    ;; add validator for atom
    (def y (atom 1 :validator (partial > 5)))
    ;;=> #'chapter06.concurrency/y
    (swap! y (partial + 2))
    ;;=> 3
    (swap! y (partial + 2))
    ;;=> IllegalStateException Invalid reference state clojure.lang.ARef.validate (ARef.java:33)

    )



;; Using CAS operation
(defn cas-test! [my-atom interval]
  (let [v @my-atom u (inc @my-atom)]
    (println "current value = " v ", updated value = " u)
    (Thread/sleep interval)
    (println "updated "
             (compare-and-set! my-atom v u))))
;;=> #'chapter06.concurrency/cas-test!
(comment
  (def x (atom 1))
  (do (cas-test! x 20)
      (cas-test! x 30)))

;;=> current value = 1 , updated value = 2
;;=> updated true
;;=> current value = 2 , updated value = 3
;;=> updated true



;; atom 放在future里并发就出错了
(comment
  (do
    (def x (atom 1))
    (future (cas-test! x 20))
    (future (cas-test! x 30))))
```



swap! 介绍



```clojure
(swap!
  donation-count ;; the Atom
  inc            ;; the function
  )
```

`swap!` takes the current value of the Atom, calls the function on it (in this case, `inc`), and sets the new value. However, just before setting the new value, it checks to make sure the old value is still in there. If it's different, it starts over. It calls the function again with the new value it found. It keeps doing this until it finally writes the value. Because it can check the value and write it in one go, it's an atomic operation, hence the name. That means the function can get called multiple times. That means it needs to be a **pure function**

> 乐观锁模式检查，重复执行. 



```clojure
(swap! donation-count + 10)
```

解释：

```clojure
(swap!
  donation-count ;; the Atom
  +              ;; the function
  ;;             ;; current value of Atom goes here!
  10             ;; the second argument
  ;;             ;; the third argument
  ;;             ;; the fourth argument, etc
  )
```

> the current value of the Atom gets put as the **first argument**. (见swap java代码)



看下swap!的源代码：

```clojure
(defn swap!
  "Atomically swaps the value of atom to be:
  (apply f current-value-of-atom args). Note that f may be called
  multiple times, and thus should be free of side effects.  Returns
  the value that was swapped in."
  {:added "1.0"
   :static true}
  ([^clojure.lang.IAtom atom f] (.swap atom f))
  ([^clojure.lang.IAtom atom f x] (.swap atom f x))
  ([^clojure.lang.IAtom atom f x y] (.swap atom f x y))
  ([^clojure.lang.IAtom atom f x y & args] (.swap atom f x y args)))


```

```java
public Object swap(IFn f, Object arg) {
	for(; ;)
		{
		Object v = deref();
        // 这里v就是atom的current value, 没错， 是作为第一个参数
		Object newv = f.invoke(v, arg);
		validate(newv);
		if(state.compareAndSet(v, newv))
			{
			notifyWatches(v, newv);
			return newv;
			}
		}
}
```





## STM using ref and dosync (alter , commute, ensure)

### ref

When should you use it？

If we look at an Atom, it only holds one value. That value can be complex (a nested collection, for instance), but it's just one value. If you have two Atoms, there's no way to make sure they stay in relationship. For example, you can't make one Atom always be twice the value of another. Why? Because some thread could read the Atoms after you change one but before you change the other.

Refs solve this problem. You can read from and modify multiple Refs inside of a transaction. Observers on the outside of the transaction cannot see the intermediate values. Nice!

> atom 只能有一个值， 最多是一个嵌套的复杂的值， 但仍然只能有一个； 你做不到一个atom始终是另一个atom的两倍， 因为他们随时被其他某一个线程修改. 

> ref可以做到， 可以在一个事务里修改多个ref， 并且事务外的观察者看不到ref的中间状态值. 



捐款、统计捐款次数例子：

```clojure
(def total-donations (ref 0))

(def count-donations (ref 0))

;; start 9 people collecting money
(dotimes [_ 9]
  (doto (Thread. (fn []
                   ;; go collect $10
                   ;; ...
                   (dosync
                     ;; record $10
                     (alter total-donations + 10)
                     ;; record one donation
                     (alter count-donations inc))
                   ;; do it again
                   (recur)))
    .start))

;; start one person tweeting the total
(doto (Thread. (fn []
                 ;; wait 100 seconds
                 (Thread/sleep 100000)
                 (tweet (str "We collected $" @total-donations " total!"))))
  .start)

;; start one person tweeting the average
(doto (Thread. (fn []
                 ;; wait 100 seconds
                 (Thread/sleep 100000)
                 (when (pos? @count-donations)
                   (tweet (str "Average donation: $" (double (dosync (/ @total-donations @count-donations))))))))
  .start)
```

> As a cost for these guarantees, you have to obey a few rules. First, **no side effects** in a transaction. 
>
> The transaction **can be run multiple times**. 
>
> Also notice that like Atoms, you **can't guarantee the order**.





```clojure
;; dosync with alter
(def makoto-account (ref {:name "Makoto Hashimoto" :amount 1000}))
(def nico-account (ref {:name "Nicolas Modrzyk" :amount 2000}))

(defn transfer! [from to amount]
  (dosync (println "transfer money from " (:name @from) " to " (:name @to) " amount = " amount " begins")
          (alter from assoc :amount (- (:amount @from) amount))
          (Thread/sleep 500) (alter to assoc :amount (+ (:amount @to) amount))
          (println "Now, " (:name @from) " amount is " (:amount @from) " and " (:name @to) " amount is " (:amount @to)) ))


(comment
  (transfer! makoto-account nico-account 10))


(comment
  (do
    (future (transfer! makoto-account nico-account 200))
    (future (transfer! makoto-account nico-account 300))))



;; 使用ensure按序执行，不用竞争
;; 这不就是加锁么？
;; No, 还保证只允许在dosync的transaction之下运行， 直接reset不允许.
(defn ensure-transfer! [from to amount]
  (dosync (ensure from)
          (println "transfer money from " (:name @from) " to " (:name @to) " amount = " amount " begins")
          (alter from assoc :amount (- (:amount @from) amount))
          (Thread/sleep 500)
          (alter to assoc :amount (+ (:amount @to) amount))
          (println "Now, " (:name @from) " amount is " (:amount @from) " and " (:name @to) " amount is " (:amount @to))))

(comment
  (do
    (future (ensure-transfer! nico-account makoto-account 100))
    (future (ensure-transfer! nico-account makoto-account 200))))



;; add watcher for ref
;;
(defn add-watcher [ref]
  (add-watch ref :watcher
             (fn [_ _ old-state new-state]
               (prn "---- ref changed --- " old-state " => " new-state) )))

(add-watcher makoto-account)
(add-watcher nico-account)

(defn refined-transfer! [from to amount]

  (dosync (ensure from)
          (if (<= (- (:amount @from) amount) 0)
            (throw (Exception. "insufficient amount")))
          (alter from assoc :amount (- (:amount @from) amount))
          (Thread/sleep 500)
          (alter to assoc :amount (+ (:amount @to) amount))))


;;Let's set values of refs using ref-set:

(comment
  (do (ref-set makoto-account {:name "Makoto Hashimoto" :amount 1000})
      (ref-set nico-account {:name "Nicolas Modrzyk" :amount 2000})) )

;;=> IllegalStateException No transaction running


(comment
  (dosync (ref-set makoto-account {:name "Makoto Hashimoto" :amount 1000})
          (ref-set nico-account {:name "Nicolas Modrzyk" :amount 2000})
          ))


;; let's test concurrently

(comment
  (do
    (future (refined-transfer! makoto-account nico-account 100))
    (future (refined-transfer! nico-account makoto-account 300))))
```

> You shouldn’t execute functions with side effects within transactions, as there is no guarantee that they will be executed only once





## 简单介绍下var



We need to talk about Vars a little. But before I do, let me say this: you almost never use Vars directly as a concurrency primitive



Whenever you define a variable using `def` or `defn`, you create a Var. They're references like Refs and Atoms. That means they're mutable. You can change the value of a Var. And we use that all the time while we're doing **interactive development**. We define a function, we realize it's not quite right, so we redefine it. That redefinition changes what's called the **root value** of the Var.

> redifinition: root value of the Var. 
>
> 你在开换环境下经常干这事， 发现不对， 重新定义。  交互式开发少不了。 

In addition to the root value, Vars can have a different value per-thread. This lets different threads use different **dynamic scopes** with the same Var.







## 补充： What are concurrency primitives?

see [Quora](https://www.quora.com/What-are-concurrency-primitives)

 two models of concurrency with different primitives, that is **multithreading**** **and **actors model**

![A7BCE1A0-A52F-4E0A-8817-5E0EFEB4D776](/assets/images/2018-01/A7BCE1A0-A52F-4E0A-8817-5E0EFEB4D776.png)

> process expensive, thread cheap.

In the *multithreading model* your main concurrency primitive is a thread. A thread can be started, and after that it starts to concurrently do something that you programmed it to do



Well, the main issue with this model, like with all concurrency in general, is **synchronization**. 

Typically, threads operate with shared memory, which makes it important to have data structures that support concurrent modification[[3\]](https://www.quora.com/What-are-concurrency-primitives#EPxUg), watch out for simultaneous updates of some variable[[4\]](https://www.quora.com/What-are-concurrency-primitives#eLSsG) by different threads. 

In Java, there was the synchronization through waiting and notifications[[5\]](https://www.quora.com/What-are-concurrency-primitives#yZBeO) to waiting threads through shared objects, but it is notoriously <u>complex</u> and had some issues with <u>notifyAll</u>, so now the preferred method of using multiple threads is through an <u>ExecutorService</u>[[6\]](https://www.quora.com/What-are-concurrency-primitives#wyOQP) that deals with many common use-cases and <u>Future</u>[[7\]](https://www.quora.com/What-are-concurrency-primitives#MuejI) for returning results from a different thread.





The second model is the *actor model,* which goes further from the underlying implementation and models all of the concurrent interactions as message passing between independent actors capable of processing. Each actor can send and receive messages, create new actors, and generally make decisions on how to act, based on the received message.



![642E7C4F-8CDA-4826-AE49-AE6574E06234](/assets/images/2018-01/642E7C4F-8CDA-4826-AE49-AE6574E06234.png)



This model actually seems more robust and easier in use, for one thing because it does not assume shared memory between agents and makes them always operate **through exchange of messages**. Because of the same reason, some people find this model to be **harder to use** too. This model can also be sometimes **referred to as an event-driven model**, because everything happens because of receiving and sending events back and forth. 







## agents (send await)

agent和Actor的不同：

An Actor receives messages, does some work based on which message they receive, then listens for more messages. Agents, on the other hand, hold state like an Atom or a Ref.



agent与atom的比较：

相同点： 都不是协同的， 既不能同时修改两个agent或者atom。 

 Like Atoms, Agents are *uncoordinated*. You can't modify two Agents with any kind of guarantees. The difference is all about which thread does the work. When you `swap!` an Atom, the processing happens in the current thread. The thread keeps retrying the computation until it successfully saves to the Atom (or throws an exception). But everything you do to an Atom happens in the current thread.

> swap! atom是在当前线程执行操作， send agent是在另一个线程。 
>
> `send` will process the computation in a thread pool.

Calling [`send`](https://clojuredocs.org/clojure.core/send) on an Agent, on the other hand, runs the computation on another thread. The call to `send` returns immediately after adding a job to a work queue that will be processed by a thread pool. So it's like an Atom, but stuff happens on another thread.





send 与 send-off:

 If your task will take a long time--a long computation or I/O--you could quickly overwhelm all of the threads in the pool and keep them too busy to take on more work. If your function does do lots of work, Clojure gives you a function called [`send-off`](https://clojuredocs.org/clojure.core/send-off), which runs each task in its own thread. Use that if you're doing I/O or a lot of computation.

> send是在线程池里执行， 即公用线程
>
> send-off： 如果计算工作非常大， 那就每个任务拥有一个独立的thread.  （合理， 不用互相等待共享线程了）。 



```clojure
(def sum (agent 0)) ;; create an agent initialized to 0

(def numbers [0 9 3 4 5 5 4 44 4 2 5 6 7 775 ...])

(doseq [x numbers]
  (send sum + x)) ;; add x to the current value

(await sum) ;; wait until all sent actions are done

(println @sum) ;; should have the answer
```

> It might be easy to think that this happens in parallel. Even though it's happening on multiple threads, it's not parallel. Each Agent has its own queue of tasks, and they are done in the order they are received, one at a time. So it's not parallel. If you want parallelism with Agents, you have to have many Agents.
>
> 如果只有一个agent并不能做到并行， 因为任务是放到队列里按顺序执行的. 



How can we make this sum parallel? Easy. Just make multiple Agents in a loop.



```clojure
;; make 10 agents initialized to zero
(def sums (map agent (repeat 10 0)))

(def numbers (range 1000000)) ;; one million numbers

;; loop through all numbers and round-robin the agents
;; 后面的(map .. 返回的是) [x agent] 这样的vector结构
(doseq [[x agent] (map vector numbers (cycle sums))]
  (send agent + x))

;; wait at most 10 seconds
(apply await-for 10000 sums)

;; sum up the answers in all ten agents
(println (apply + (map deref sums)))
```

In this example, all agents get the same number of tasks. What happens if some tasks take longer than others? That means that some Agents will be idle while others are still working. How can you prevent that?



The answer, predictably, is to add another level of indirection.

Instead of round-robin, we should add our numbers to a queue that the agents pull from when they're ready for more work.



;; 这部分代码有点难度， 还没看明白. 

```clojure
;; make 10 agents initialized to zero
(def sums (map agent (repeat 10 0)))

(def numbers (agent (range 1000000))) ;; one million numbers in an agent

(defn dequeue-and-add [sum-agent]
  (letfn [(add [current-sum x]
            ;; do the addition
            (let [new-sum (+ current-sum x)]
              ;; when we're done, schedule the next dequeue
              (send numbers dequeue)
              ;; return the new value of the Agent
              new-sum))
          (dequeue [xs]
              ;; check if there's more to do
              (when (not (empty? xs))
                ;; send the first number to the Agent
                (send sum-agent add (first xs)))
                ;; return the other numbers for other Agents
                (rest xs))]
    (send numbers dequeue)))

;; start all 10 Agents working
(doseq [sum-agent sums]
  (dequeue-and-add sum-agent))

;; wait for all the numbers to be cleared from the queue
(loop []
  (when (seq @numbers)
    (Thread/sleep 1000)
    (recur)))

;; sum up the answers in all ten agents
(println (apply + (map deref sums)))
```







;; 之前的代码 

```clojure
;; creating agent
(def simple-agent (agent 0))

;; updating agent by send method
(send simple-agent inc)

;;
(def makoto-agent
  (agent {:name "Makoto" :location [100 200]}))


(defn move [a dx dy t]
  (println "moving takes " t "msecs")
  (Thread/sleep t)
  (assoc a :location
           [(+ ((:location a) 0) dx)
            (+ ((:location a) 1) dy)]))


(comment
  (do
    ;; send 后面的参数 第一个是agent， 后面就是method以及对应的参数
    ;; 注意move的第一个参数是agent, 类似于 ->
    (send makoto-agent move 10 20 1000)
    (println makoto-agent)
    (await makoto-agent)
    (println makoto-agent)))
```

> Agents are the way to go if you want to cause **side effects** in a transaction; since, they will only be executed after the transaction has completed successfully.

agent和STM不一样， 允许有副作用， 因为它只在事务成功后执行。 



Agents also run in **separate thread pools**, there are two functions that you can use to fire an agent:

- send: This executes your function in an implicit thread pool 
- send-off: This tries to execute your function in a new thread but there’s a change, it will reuse another thread



## Future

A Future is similar to a Promise. The main difference is that Futures will evaluate an expression for you in another Thread. **You don't have to set the thread up**. Here's how the same thing works using [`future`](https://clojuredocs.org/clojure.core/future)

> Future 自动帮你新建一个thread， 换言之， Promise是在当前thread跑。 



```clojure
(def the-answer (future
                   ;; do a lot of work in a new thread
                   ;; ...
                   ;; then deliver the answer
                   ;; the value of the last expression is delivered
                   42))

;; in the main thread, do some work
;; ...
;; then get the answer (which blocks until the answer is done)
(println (deref the-answer))
```



Future的异常问题：

One thing that trips up people new to Futures is that they **swallow exceptions**. If the code you run in the Future throws an exception, you won't hear about it until you `deref` it. When you `deref` it, the **exception will be <u>thrown again</u> in the current thread**.



```clojure
;; the Exception gets thrown but stored in the Future
(def f (future (throw (Exception. "Hello from the future!"))))

(deref f) ;; this will throw the Exception
```







## 一句话介绍deref

It stands for **dereference**， both Promises and Futures are types of references. They're not the values themselves. **They're pointers to the values**. 



We can also abbreviate `deref`by prepending a `@` to the reference, like this:



In Clojure speak, when a promise has been delivered, we say it is *realized*. There's even a function called `realized?` to check if it has an answer



deref增加timeout参数：

```
;; wait four seconds (4,000 ms)
;; if we don't have an answer by then, deref will return :cheeseburger
(deref the-answer 4000 :cheeseburger)
```



## promise and deliver

> Promises are simple abstractions that don’t impose strict requirements on you; you can use them to calculate the result on some other thread, light process, or anything you like.

> In Java, there are a couple of ways to achieve this; one of them is with futures (java.util.concurrentFuture)

> Promises are only calculated once and then cached.



> Calling `promise` will make a coupon for a value you promise to deliver some time in the future. Usually you are going to **calculate that value in another thread**.

```clojure
(defrecord Order [name price qty])


;; 统计总价
(defn merge-products [m1 m2]
  {:total-price (+ (* (.price x) (.qty x)) (* (.price y) (.qty y)))}
  [m1 m2])


;; 组装完产品后发货， 等待x，y完毕，即x和y的deliver
(defn ship-products [x y z]
  (deliver z (merge-products @x @y))
  (println "We can ship products " @z))



(def product-a (promise))

(def product-b (promise))

(def shipping-ab (promise))

(future (ship-products product-a product-b shipping-ab))

(deliver product-a (->Order "book" 10.1 5))

(deliver product-b (->Order "pencil" 2.1 10))
```



### promise vs future

> reference: [stackeroverflow](https://stackoverflow.com/questions/4623536/how-do-clojure-futures-and-promises-differ)



 **promise**

1. You create a `promise`. That promise object can now be passed to any thread.
2. You continue with calculations. These can be very complicated calculations involving side-effects, downloading data, user input, database access, other promises -- whatever you like. The code will look very much like your mainline code in any program.
3. When you're finished, you can `deliver` the results to that promise object.
4. Any item that tries to `deref` your promise before you're finished with your calculation will block until you're done. Once you're done and you've `deliver`ed the promise, the promise won't block any longer.

**future**

1. You create your future. Part of your future is an expression for calculation.
2. The future may or may not execute concurrently. It could be assigned a thread, possibly from a pool. It could just wait and do nothing. From your perspective *you cannot tell*.
3. At some point you (or another thread) `deref`s the future. If the calculation has already completed, you get the results of it. If it has not already completed, you block until it has. (Presumably if it hasn't started yet, `deref`ing it means that it starts to execute, but this, too, is not guaranteed.)



---

Both Future and Promise are mechanisms to communicate result of asynchronous *computation* from Producer to Consumer(s).

In case of **Future** the *computation* is defined at the time of Future creation and async execution begins "ASAP". It also "knows" how to spawn an asynchronous computation.

In case of **Promise** the *computation*, its *start time* and [possible] *asynchronous invocation* are decoupled from the delivery mechanism. When *computation* result is available Producer must call `deliver` explicitly, which also means that Producer controls *when* result becomes available. 

For **Promises** Clojure makes a design mistake by using the same object (result of `promise` call) to both produce (`deliver`) and consume (`deref`) the result of *computation*. These are two very distinct capabilities and should be treated as such.



## future (新建一个thread)

> 是java.util.concurrent.Future的Clojure简化版



与promise不同点：

> using futures everything will run in a different thread automatically.



Futures are also cached



future vs promise: 缺点 （黑盒， 不够灵活）

- You have <u>less flexibility</u>; you can only run futures in a predefined thread pool. 
- You should be careful if your futures take too much time, they could end up NOT running because the **implicit thread pool has a number of threads available**. If they are all busy some of your tasks will end up queued and waiting.





## pmap , pcalls and pvalues





## actor







## Using Spyglass and Couchbase to share state between JVMs





## core.async

> core.async is yet another way of programming concurrently; it uses the idea of **lightweight threads and channels** to communicate between them.



### Goblocks

Goblocks are similar to **threads**; you just need to remember that you can create goblocks freely. There can be **thousands of running goblocks** in a single JVM



### channel

> suggest to use channel to communicate with goblocks

```clojure
(let [c (chan)]
  (go (println (str "The data in the channel is" (<! c))))
  (go (>! c 6)))
```



- chan: This function creates a channel and the channels can **store** some **messages** in a buffer. If you want this functionality, you should just pass the size of the buffer to the chan function. If no size is specified, the channel can store only one message.
- \>!: The **put** function must be used within a goblock; it receives a channel and the value you want to publish to it. This function **blocks**, if a channel’s buffer **is already full**. It will block until something is consumed from the channel.
- <!: This takes function; this function must be used within a **goblock**. It receives the channel you are taking from. It is blocking and if you haven’t published something in the channel it will **park until there’s data available**.

> 读和写都是阻塞的
>
> \>! 和 <! 都只能在goblocks里使用



另外两个读写操作是:

- \>!! 特点是可以不用在goblocks里执行， 而是任何地方
- <!! 特点也是不用在goblocks里执行，而是任何地方



