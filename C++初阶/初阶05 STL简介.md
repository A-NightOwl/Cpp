**本章重点**
1. 什么是STL
2. STL的版本
3. STL的六大组件
4. STL的重要性
5. 如何学习STL
6. STL的缺陷


# 1.什么是STL
STL(standard template libaray-标准模板库)：**是C++标准库的重要组成部分**，不仅是一个可复用的组件库，而且**是一个包罗数据结构与算法的软件框架。**

# 2.STL的版本
## 2.1 原始版本
>Alexander Stepanov、Meng Lee 在惠普实验室完成的原始版本，本着开源精神，他们声明允许任何人任意运用、拷贝、修改、传播、商业使用这些代码，无需付费。唯一的条件就是也需要向原始版本一样做开源使用。 HP 版本--所有STL实现版本的始祖。

## 2.2 P.J.版本
>由P. J. Plauger开发，继承自HP版本，被Windows Visual C++采用，不能公开或修改，缺陷：可读性比较低，符号命名比较怪异。

## 2.3 RW版本
>由Rouge Wage公司开发，继承自HP版本，被C+ + Builder 采用，不能公开或修改，可读性一般。

## 2.4 SGL版本
>由Silicon Graphics Computer Systems，Inc公司开发，继承自HP版 本。被GCC(Linux)采用，可移植性好，可公开、修改甚至贩卖，从命名风格和编程 风格上看，阅读性非常高。
>
>**我们后面学习STL要阅读部分源代码，主要参考的就是这个版本。**

# 3.STL的六大组件

![请添加图片描述](https://i-blog.csdnimg.cn/direct/9c9fa4e2e4254482beab04d60bd2bc35.png)

其中"空间配置器"即前文"内存池"

我们先会去学算法和容器，容器大多数就是我们学习的数据结构

# 4.SLT的重要性
## 4.1 笔试中
[二叉树层序打印](https://www.nowcoder.com/practice/445c44d982d04483b04a54f298796288?tpId=13&tqId=11213&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)  
[重建二叉树](https://www.nowcoder.com/practice/8a19cbe657394eeaac2f6ea9b0f6fcf6?tpId=13&tqId=11157&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)  
[两个栈实现一个队列](https://www.nowcoder.com/practice/54275ddae22f475981afa2244dd448c6?tpId=13&tqId=11158&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

像这样的题，我们直接使用STL库来做会比较容易，就不需要我们自己去写那些数据结构之类的

在一些蓝桥杯的比赛中，一般都是C++组、Java组，就没有C语言组，这就是因为C语言缺少像STL这样的库，写起来需要从底层慢慢搭建，比较繁琐

## 4.2 在面试中
比如以下问题：  
你项目里有空间配置器,你给我讲讲空间配置器和智能指针有什么联系吗?  
智能指针了解多少,讲讲auto_ptr.  
为什么C++11删掉了auto_ptr,他有什么缺陷吗?  
C++11里有nullptr,这和NULL有什么区别吗?  
讲讲vector和list,再讲讲两个区别  

# 5.如何学习STL
![请添加图片描述](https://i-blog.csdnimg.cn/direct/96ab09b26ae843938adbff176ef91388.png)

简单总结一下：学习STL的三个境界：**能用**，**明理**，**能扩展** 。


# 6.STL的缺陷
1. STL库的更新太慢了。这个得严重吐槽，上一版靠谱是C++98，中间的C++03基本一些修订。C++11出来已经相隔了13年，STL才进一步更新。
2. STL现在都没有支持线程安全。并发环境下需要我们自己加锁。且锁的粒度是比较大的。
3. STL极度的追求效率，导致内部比较复杂。比如类型萃取，迭代器萃取。
4. STL的使用会有代码膨胀的问题，比如使用vector/vector/vector这样会生成多份代码，当然这是模板语法本身导致的。


