# Java Memory Model
Understanding java memory model is important to understand how threads in java interact with each other through memory. On modern platforms, code we write may not be executed in the same order as it is written, In order to acheive maximum performance, compiler, processor would reorder them, and this is what called as compiler optimization, writing and reading shared field objects do not give guaranteed consistency due to memory reorder by compiler.

## Synchronization

Now we have understood why compiler would reorder instrucutions to improve performance, When multiple threads communicate with each other, They would do by sharing fields or object references which contains those fields. This form of thread communication is extremely efficient, it would cause two kinds of errors thread interference, memory consistency errors. So, how can we avoid these errors when thread communication takes place, synchronization comes into picture, Synchronization is a keyword in java that can be used to prevent these kind of errors. 

However, synchronization can introduce thread contention when multiple threads try to access the same resource simultaneously, causes one or more threads to run slowly. In java, synchronization can be used on method level or block level. When using synchronized method or block, only one thread can acccess and other thread would wait for this to finish. To synchronize threads, java uses monitors which are high level mechanism of allowing only one thread to execute synchronized region of code, each object and class in java has it's own lock, When we synchronize a method or block it uses that lock(monitor) associated with object or class.

### What exactly happens during synchronization that gives consistency?

Consider the below code to explain what will happen when multiple threads access Counter object to increment and decrement. UnsafeCounter would produce inconsistent results whereas SafeCounter would return correct results due to synchronization on method levels.

Incrementing a count is not done in 1 step. Thread reads counter field from memory, increment it using processor and store it in processor cache before updating to the memory. When two or more threads call increment method in UnsafeCounter, thread1 reads counter as 0, Since it s not synchronized thread2 also reads the same value as 0, now both threads increment it as 1 update it to the counter which is incorrect.
        
In SafeCounter methods, When thread1 enters into synchronized block, thread2 has to wait for the thread1 to release the lock. thread1 reads counter from memory and increment it using processor, when thread1 exits synchronized block processor cache are flushed back to main memory so writes made by this thread is visible to all other threads. now thread2 acquires the lock and reads the updated value from memory.

***When thread exits synchronized block or method, processor cache is flushed back to main memory so that other threads see the updated value***


```
static class UnsafeCounter {

    int count = 0;

    public void increment() {
        this.count++;
    }

    public void decrement() {
        this.count--;
    }
}

static class SafeCounter {
    int count = 0;

    public synchronized increment() {
        this.count++;
    }
        
    public synchronized decrement() {
        this.count--;
    }
}
```