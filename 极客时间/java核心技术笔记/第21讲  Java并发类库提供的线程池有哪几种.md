## 第21讲 | Java并发类库提供的线程池有哪几种？ 分别有什么特点？

1. Executors 目前提供了 5 种不同的线程池创建配置：
   - newCachedThreadPool()，它是一种用来处理大量短时间工作任务的线程池，具有几个鲜明 特点：它会试图缓存线程并重用，当无缓存线程可用时，就会创建新的工作线程；如果线程 闲置的时间超过 60 秒，则被终止并移出缓存；长时间闲置时，这种线程池，不会消耗什么 资源。其内部使用 SynchronousQueue 作为工作队列。
   - newFixedThreadPool(int nThreads)，重用指定数目（nThreads）的线程，其背后使用的 是无界的工作队列，任何时候最多有 nThreads 个工作线程是活动的。这意味着，如果任务 数量超过了活动队列数目，将在工作队列中等待空闲线程出现；如果有工作线程退出，将会 有新的工作线程被创建，以补足指定的数目 nThreads
   - newSingleThreadExecutor()，它创建的是个 ScheduledExecutorService，也就是可以进 行定时或周期性的工作调度。工作线程数目被限制为 1，所以它保证了所有任务的都是被顺 序执行，最多会有一个任务处于活动状态，并且不允许使用者改动线程池实例，因此可以避 免其改变线程数目。
   - newScheduledThreadPool(int corePoolSize)，同样是 ScheduledExecutorService，区别 在于它会保持 corePoolSize 个工作线程
   - newWorkStealingPool(int parallelism)，这是一个经常被人忽略的线程池，Java 8 才加入 这个创建方法，其内部会构建ForkJoinPool，利用Work-Stealing算法，并行地处理任务， 不保证处理顺序。
2. 