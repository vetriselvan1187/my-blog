
# Async Future

A Future represents the result of asynchronous computation. In Java, asynchronous tasks such as background tasks, long running tasks or taks that run in separate thread would use Runnable or Callable. We use runnable when a task that do not return any result during computation and use callable when a task returns a result. 


```

Callable<Integer> callable = () -> {
	int random = new Random().nextInt(10) * 100;
	System.out.println("Preparing to execute");
	Thread.sleep(random);
	System.out.println("Executed - " + random);
	return random;
};
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Integer> future = executor.submit(callable);
try {
	Integer value = future.get(2, TimeUnit.SECONDS);
	System.out.println("Value is " + value);
} catch (InterruptedException | ExecutionException | TimeoutException e) {
	e.printStackTrace();
}
executor.shutdown();

```

## CompletableFuture
CompletableFuture is used in asynchronous computation to complete background tasks, long running tasks ..etc. Future is the reference to the result of asynchronous computation, future has some limitations, future can not be manually completed, can not process further action on result without blocking, multiple futures can not be chained together and no exception handling. CompletableFuture implements Future and CompletionStage interface and provides a methods for creating, chaining, combining multiple futures together. 

CompletableFutre.get() method blocks for the result in aynchronous computation which we can be manually completed  with CompletableFuture.complete("Result").

CompletableFuture uses global thread pool (ForkjoinPool.commonPool) to execute run tasks asynchronously. runAsync() method is used to run a task that dont return any result, supplyAsync() method is used to run run task that returns a result, supplier is a functional interface which is passed to supplyAsync to process and return result. supplyAsync is chained with thenApply, thenAsync, thenRun to process the result asynchronously.


```
// Transformation is chained as thenApply when supplier is finished
CompletableFuture<String> text = CompletableFuture.supplyAsync(()-> {
	// sleep 10s 
	return "completed text"
}).thenApply( textString -> {
	return "Hello "+textString;
}).thenApply( helloText -> {
	return "This is completed "+helloText;
});

System.out.println(text.get());

// runAsyc to process result asynchronously
CompletableFuture.runAsyc(()-> {
	return "Result";
}).thenAccept(result -> {
	System.out.println("process result");
}).thenRun(() -> {
	System.out.println("computation finished");
});

```

