
# Threads and Runnables

In this topic, we are going to understand how threads communicate with each other in a multithreaded java environment.

JVM (Java Virtual Machine) has been designed to support concurrent computing. So what is it ? 
Concurrent computing allows us to execute programs with overlapping time periods. Instead of exeuting programs sequentially in which one program completes before the next one starts, each program would run for predefined time slice. In a single core processor, running multiple threads or opening multiple programs would cause them to run with overlapping time periods. This should not to be confused with parallel computing where multiple process would effectively utilize multiple processors. 

In Java, all execution takes place in the context of threads. JVM runs as single process, concurrent programming in java is mostly concerned with multiple threads. So, when we talk about concurrent programming in java, we mostly would talk about running multiple threads in JVM.Each thread in jvm has its own stack and execution path and its very lightweight compared to process. When multiple threads try to access shared resources such as objects, we must ensure that it's properly synchronized to prevent any undesirable effect on programming behaviour. Threads share the process resources such as open files, memory for efficiency. 

Every java application starts with one thread called main thread. Main thread has the ability to create additional threads such as Runnable or Callable. Runnable does not return any result whereas Callable would return result, both of them are designed to be executed in separate thread. Threads in jvm can be scheduled on different cpu core or can use time slicing on single core processor or can use time slicing on multiple processor, it depends on how JVM map this to native os threads.

## Runnable Implmentation

```
    public class ThreadRunnable implements Runnable {

        @override
        public void run() {
            System.out.println("Thread runnable got executed");
        }

        public static void main(String[] args) {
            new Thread(new ThreadRunnable()).start();
        }
    }
```