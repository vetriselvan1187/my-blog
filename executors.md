# Executors
   
Executors helps us to decouple task submission from execution. We can submit task to executor so it would take care of executing task and return a future which can be used to track progress of one or more asynchronous task. Thread creation is expensive and difficult to manage, avoid creating threads for running tasks if possible. Executors solve this problem by creating and managing pool of threads. There are 6 types of executors available.

- Single Thread Executor : executor has only one thread to process tasks
- Cached Thread Pool : suitable for long running performance tasks, thread limit is unbound. 
- Fixed Thread Pool : Bounded thread limit, maintains a same thread pool size.
- Scheduled Thread Pool : thread pool to execute tasks at fixed delay or at specific time.
- Single-Thread Scheduled Pool : similar to the scheduled thread pool, but execute only one active task at time.
- Work-Stealing Thread Pool : Based on Fork/Join framework with available processors, it applies the work stealing algorithm for balancing tasks.

There are 2 types of tasks 
- execute : Executes without giving feedback
- submit : Returns a future.

ShutDown() - waits for tasks to terminate and release resources.

ShutDownNow() - Try to stops all executing tasks and returns a list of not exececuted tasks

```

// cached thread pool
var cachedThreadPool = Executors.newCachedThreadPool();
var uuids = new LinkedList<Future<UUID>>();

for (int i = 0; i < 10; i++) {
	var submittedUUID = cachedThreadPool.submit(() -> {
		var randomUUID = UUID.randomUUID();
	    System.out.println("UUID " + randomUUID + " from " + Thread.currentThread().getName());
		return randomUUID;
	});
	uuids.add(submittedUUID);
}

cachedThreadPool.execute(() -> uuids.forEach((f) -> {
	try {
		System.out.println("Result " + f.get() + " from thread " + Thread.currentThread().getName());
	} catch (InterruptedException | ExecutionException e) {
		e.printStackTrace();
	}
}));
cachedThreadPool.shutdown();

try {
	cachedThreadPool.awaitTermination(4, TimeUnit.SECONDS);
} catch (InterruptedException e) {
	e.printStackTrace();
}

```