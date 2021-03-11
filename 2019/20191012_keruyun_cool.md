
[客如云](http://wap.keruyun.com/)
-------

### 1. Java 泛型，上下界
extends 上界只负责放，super 下界只负责取

### 2. String StringBuilder StringBuffer

### 3. Retrofit了解的怎么样，有几种类型的拦截器？
普通的拦截器，network拦截器，注意retry是在拦截器的第一个位置

### 4. 讲讲handler，looper.loop()为什么没导致主线程阻塞
这块的话，面试官没理解到我的意思，其实他跟我都知道原因，只是我回答的不是他心中的句子，出了一点分歧


### 5. 序列化，Parcelable/Serialisable 区别？如何让 Parcelable 的write 那一串变得简洁？
kotlin 是需要 data-class，然后实现 Parcelable


### 6. HTTPS 了解过嘛，建立连接的过程是怎样的
RSA + AES，协商秘钥并进行对称加密传输

### 7. HashMap put 原理，ConcurrentHashMap 如何保证多线程间数据的同步
ConcurrentHashMap 通过CAS机制，在处理冲突的时候，也会有synchronized处理


### 8. synchronized 和 volatile 关键字区别


### 9. synchronized 加在方法和代码块上有什么不同？静态方法呢？哪些情况下会用 wait/notify？
wait 和 notify 的场景，生产者/消费者的关系


### 10. Activity的启动模式
standard/singleTop/singleInstance/singleTask


### 11. 接口和抽象类的区别


### 12. View 事件分发


### 13. 算法: 数组相邻元素去重，[-1,1,1,-1,3,2,5] 返回 [3,2,5]，15分钟
15分钟显然没写出来，我以为是要完整的代码
后面只需要你的思路就可以了，这我觉得蛮好的
思路:
1. 检查数组中的元素，有相同的移除
2. 递归检查，然后移除，直到没有相邻元素相同

直接用递归的话，显然不是最优的，需要第一遍将所有相邻的都找出来


<hr/>

### 1. 用一句话描述RxJava的怎么实现线程调度的
认怂，转Retrotit

### 2. Retrofit 的convertor，如果add多个convertor怎么处理，如果不加会发生什么？

### 3. 组件化，讲讲需要注意的是那些，或者说那些你觉得比较重要
界面路由和数据传递
由于我们选的是Androim，是通过在App里实例化，然后其他module在base暴露service，供其他模块调用
然后，"我的"和"首页"都会用到账户信息，你们把我的作为一个module，你觉得这样合适吗，你看京东淘宝等等
我们吧账户信息单独抽了一个account这样的module，这样避免了两个modle之前的强耦合


### 4. 你对哪一块比较了解，视图、媒体、网络

<hr/>

### 1. 居然自我介绍


### 2. 你毕业3年就换了3家公司，都是什么原因呢？你怎么看你当初的选择，为什么要选xx，我是很不看好你选xx公司


### 3. 在工作中遇到过哪些问题，怎么解决的？花了多长时间，有没有更长的？
感觉是必问问题


### 4. 在团队中担任什么样的角色


### 5. 对自己的评价是怎样的


### 6. 工作中最失望的是什么，有没有失望过？是哪方面比较失望？


### 7. 你从这两次求职选择学到了哪些？有哪些成长，现在关注哪些？

