**原力字节**，竟然没有他们发现官网...


序号不是严格提问顺序
-----


### 1. StringBuilder 和 StringBuffer， 1.8 以后处理方式有变化吗？
builder线程不安全<br>
TODO: 1.8以后，需要看看代码有什么不同, 不知是不是问 扩容问题


### 2. SharedPreferences, commit 失败遇到过吗，该如何处理？
不知道他说的是 commit 不生效还是 commit 的返回值为 `false`
1. 不生效的话，可能是因为通过 sp.edit().commit(), edit() 每次调用都是新的，所以没不生效
2. 如果是返回`false`的话，原因就比较多了，如果是 MemoryCommitResult.result = false 需要去看 writeToFile 的log


### 3. Synchronize/Volatile
volatile 内存可见性，指令重排序，不阻塞线程；仅能作用在变量上<br>
synchronize 线程互斥，能保证操作的原子性；作用在方法/代码块；底层通过对象头信息来表明是否有锁<br>
`*`transient 保证不被序列化<br>


### 4. 讲讲 乐观锁和悲观锁
synchronized / compareAndSwipe


### 5. 公平锁和非公平锁 AQS
acquire tryAcquire

公平锁都是通过 `tryAcquire` 去尝试获取锁，而非公平锁是先 `acquire`，如果失败才去 `tryAcquire`


### 6. MediaPlayer 遇到不支持的格式的视频，怎么处理
TODO: 还没接触过比较底层的 编解码 过程


### 7. Activity之间的跳转，A -> B 生命周期
A: onPause<br>
B: onCreate -> onStart -> onResume<br>
A: onStop<br>
为什么A要先Pause呢？<br>
因为需要将A站停，如果B是一个电话界面，A是播放音乐，如果不将A暂停，就不能正常接电话了<br>


### 8. Kotlin const val 在类外和伴生对象里的 const val 区别
类外 top-level，在整个包内可见，包内的任意对象里都能访问到这个常量<br>
companion object 里的，需要通过class去访问，也侧面说明了`伴生对象`这个名词<br>
TODO: 得看看生成的字节码是什么样的


### 9. HashMap put 过程
先计算hash，key.hashCode() 高16位 与 低16位进行异或得到真正的 hash，然后与table.length - 1 相与 得到index<br>
看index是否为空，为空直接设置为value<br>
    如果不为空<br>
--->1. 再看hash是否相同，通过 `==` 和 `equals` 比较key是否一致，一致的话，更新value为新的值<br>
--->2. 再看是不是红黑树，是的话直接putTreeVal<br>
--->3. 普通链表，遍历链表并计数，超过8个节点将会看是否需要转成红黑树；如果tab的length < 64 的话会进行扩容，否则转为红黑树<br>
--->4. 如果再链表中找到节点，同样会通过 `==` 和 `equals` 判断是否是同一个key<br>


然后再看，++size是否到了 threshold = `loadFactor * capacity`，到了进行扩容，扩容过程<br>
--->1. 创建新的tab<br>
--->2. 如果旧节点不为空，且没有链，直接计算新的index并设置值<br>
--->3. 如果旧节点是红黑树，进行分离<br>
--->4. 如果旧节点是链表，处理方式比较巧妙，`hash & oldCap = 0` 就保持位置不动。否则移动到新的扩展区，index就是当前的 `oldIndex + oldCap`<br>
       相当于链表被均匀分到了tab的两部分<br>


Java 1.7 为啥要用头插法？


### 10. Socket 的连接和断开
连接：3次握手<br>
Client --------------------------------------- Server<br>
SYN=1,seq=x               --------->    // 确认客户端发送正常<br>
客户端确认服务端发送接收正常 <---------    `SYN=1,ACK=1,ack=x+1,seq=y`<br>
ACK=1,ack=y+1,seq=x+1     --------->    // 确认客户端接收正常<br>


断开：4次挥手<br>
Client ---------------------------------------- Server<br>
FIN=1,seq=w               ---------><br>
                          <---------   ACK=1,ack=w+1,seq=v<br>
                          <---------   FIN=1,ACK=1,ack=w+1,sep=u<br>
ACK=1,ack=u+1,seq=w+1     ---------><br>
timewait=2MSL, 因为服务端需要确认客户端收到了，如果没有收到客户端的回应，服务端会进行超时重发，会导致永远关不掉<br>
[掘金-TCP-连接](https://juejin.im/post/5b1d34eb6fb9a01e7d5c3e25)


### 11. 你最擅长哪块？
(因为很多东西停留在使用，没有很特别的深入细节) 就说 Retrofit 和 `Glide` 看的多一些 (但只问了 Retrofit，应该细化Retrofit的某一点问就好了)


### 12. 说说 Retrofit

create通过InvocationHandler构建interface的代理类，然后通过 `loadServiceMethod` 创建一个method然后invoke，这个method是 `HttpServiceMethod`<br>
invoke 其实是通过callAdapter.adapt(new `HttpCall()`<br>

callAdapter 是在初始化Retrofit时候设置的 `RxJava2CallAdapterFactory`，创建一个call对象 flowable/observable/single 默认创建一个同步的请求<br>
然后创建一个包含 `Call` 的 Observable，当subscribe的时候才真正开始请求。就是执行 httpcall.execute(), 异步的话是 httpcall.enqueue<br>

HttpCall 并不是真正的 OkHttp 的call，realCall 是在execute/enquere的时候，判断rawCall为空，通过callFactory创建一个call<br>
callFactory是正好就是 初始化Retrofit时候设置的client `okHttpClient`, 它实现了实现CallFactory, `newCall() -> RealCall.newRealCall()`<br>
这个 realCall 就是真实的call了，通过他执行网络请求。<br>

realCall 里的execute方法再通过 `getResponseWithInterceptorChain` 创建真正的请求<br>
这里有个拦截链（责任链）<br>
就是Retrofit添加的拦截器，client.interceptors() + 重试 + BridgeInterceptor() + 缓存 + ConnectInterceptor() + CallServerInterceptor()<br>
然后就开始 chain.process()<br>

`ConnectInterceptor` 创建连接，设置socket<br>
`CallServerInterceptor` 读取请求数据，设置content-length，websocket的协议转换<br>

怎么一级一级返回呢，这个链很特别，process会返回response<br>
第一个拦截器的intercept 调第二个拦截器的intercept 又接着调第三个拦截器的 intercept，然后最后一个intercept返回后得到response，再一个一个获取上一个将response上传递<br>

call 完成就回到了Retrofit的execute，再通过 `parseResponse(response)` 处理返回结果 GSON 的转换也是在这里，responseConverter.convert() 就完成了转换<br>

responseConverter 是 converterFactories.get(i).responseBodyConverter创建在 `loadServiceMethod` 的时候传入的


### 13. MVVM - MVP 优劣
MVVM 基于databinding 不利于扩展，View的绑定了数据类，不利于复用<br>
MVP  presenter 比较重，View可复用性较强，但逻辑比较集中，维护起来相对还好，也利于编写测试用例，直接可以单独测试 M/P/V<br>
[简书-Android-MVP-测试用例](https://www.jianshu.com/p/cf446be43ae8)<br>
