# Concurrent包概述

## Executor
Executor是一个表示::执行提供的任务::的对象的接口。

## ExecutorService
ExecutorService是异步处理的完整解决方案。它管理内存中队列并根据线程可用性计划提交的任务。

## ScheduledExecutorService
ScheduledExecutorService是与ExecutorService类似的接口，但它可以::定期执行任务::。

## Future
Future用于表示::异步操作的结果::。它带有检查异步操作是否完成，获取计算结果等的方法。

## CountDownLatch
CountDownLatch（在JDK 5中引入）是一个实用程序类，它::阻止一组线程，直到某些操作完成::。

## CyclicBarrier
CyclicBarrier与CountDownLatch几乎相同，只是我们可以重用它。与CountDownLatch不同，它允许多个线程在调用最终任务之前使用`await()`方法（称为障碍条件）彼此等待。

## Semaphore
信号量被用于阻挡到物理或逻辑资源的某些部分螺纹级别的访问。信号量包含一组许可证; 每当线程试图进入临界区时，如果有可用许可证，它需要检查信号量。

## ThreadFactory
ThreadFactory充当线程（不存在）池，它根据需要::创建新线程::。它消除了大量样板编码的需要，以实现有效的线程创建机制。

## BlockingQueue
在异步编程中，最常见的集成模式之一是生产者 - 消费者模式。该的java.util.concurrent包带有一个数据结构所知道的BlockingQueue的 -它可以在这些异步情况下非常有用的。

## DelayQueue
DelayQueue是一个无限大小的元素阻塞队列，只有当元素的到期时间（称为用户定义的延迟）完成时才能被拉出。因此，最顶部的元素（头部）将具有最大量的延迟并且将最后轮询。

## Lock
Lock是一个实用程序，用于阻止其他线程访问某段代码，除了当前正在执行它的线程。

## Phaser
Phaser是比CyclicBarrier和CountDownLatch更灵活的解决方案- 用作可重复使用的屏障，动态线程数在继续执行之前需要等待。我们可以协调多个执行阶段，为每个程序阶段重用Phaser实例。