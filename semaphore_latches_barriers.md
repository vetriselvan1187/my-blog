# Semaphore - Latches - Barriers

## CountDownLatch
CountDownLatch is a class in concurrent package that allows one or more threads to wait untill set of operations being performed on other threads. CountDownLatch is initialized with a count.
The await method on latch blocks untill current count reaches zero. countDown method decrements the count and it will reach zero when all threads are completed.

```

val executor = Executors.newFixedThreadPool(3);

val countDownLatch = new CountDownLatch(3);
val runnable = () -> {
    try {
        Thread.sleep(4000);
        countDownLatch.countDown();
    } catch(InterruptedException ex) {
        ex.printStackTrace();
    }
};

executor.execute(runnable);
executor.execute(runnable);
executor.execute(runnable);

countDownLatch.await();

```

## CyclicBarrier
CyclicBarrier is a reusable synchronization barrier which allows number of threads to wait for each other untill last thread arrives at await. Once last thread reaches that barrier, all threads will continue to progress further. Unlike countdownlatch, Cyclicbarrier can be reset and reused again.   

```
val executor = Executors.newFixedThreadPool(3);
val barrier = new CyclicBarrier(3);
val runnable = () -> {
    try {
        Thread.sleep(Random.nextInt(4000));
        barrier.await();
        System.out.println("progress after barrier");
    } catch(InterruptedException ex) {
        ex.printStackTrace();
    }
};

for(int i = 0; i < 3; i++) {
    executor.execute(runnable);
}

```


## Semaphore
Semaphore is a synchronization construct which allows multiple threads to acquire and release lock at the same time. Unlike mutex where only one thread acquire and release lock, Semaphore is initialized with a count(permits), threads are allowed to acquire lock if permits are available, once the number of available permit reaches zero, threads will be blocked untill any of other thread release the permit(lock). simple example would be resource pool or threadpool implementation.

```
var executor = Executors.newCachedThreadPool();

var semaphore = new Semaphore(3);

Runnable r = () -> {
	try {
		System.out.println("Trying to acquire - " + Thread.currentThread().getName());
		if (semaphore.tryAcquire(2, TimeUnit.SECONDS)) { // acquire the permit from semaphore
			// work in progress
			System.out.println("Acquired - " + Thread.currentThread().getName());
			Thread.sleep(2000);
			System.out.println("Done - " + Thread.currentThread().getName());
		}
	} catch (InterruptedException e) {
		e.printStackTrace();
	} finally {
		semaphore.release(); // release the permit to semaphore
	}
};

for (int i = 0; i < 4; i++) {
	executor.execute(r);
}		
executor.shutdown();

```