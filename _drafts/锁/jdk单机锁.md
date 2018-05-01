jdk常用的几种锁
===
*  可重入互斥锁: Lock lock = new ReentrantLock()
*  信号量: Semaphore semaphore = new Semaphore(3);
*  读写锁:ReadWriteLock lock = new ReentrantReadWriteLock();
*  倒计数锁: CountDownLatch latch = new CountDownLatch(4);
*   栅栏锁:CuclicBarrier barrier =  new CyclicBarrier()



