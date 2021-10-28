# Atomics

In this post, have to discuss about the difference between atomic and volatile and synchronization.

## Volatile
When we apply volatile to a variable, it guarantees that read and writes establish a happens-before-relationship, much like acquiring and releasing mutex. Any changes done on volatile variable is immediately visible to all other threads. Incrementing counter with volatile keyword ensures that changed value is immediately updated to memory and subsequent reads by other threads also will see the latest value. volatile variable don't provide atomicity when multiple threads write and read on the same variable. 

**
    When to use: One thread modifies the data and other threads read the latest value of data. other threads will not do any update.
**

```
    private volatile int counter = 0;

    // updated by one thread
    public void increment() {
        count++;
    }

    // read by multiple threads
    public int get() {
        return count;
    }

```

## Atomic variables

Atomic classes in java support atomic operation on single variables. ex: (java.util.concurrent.atomic.*), It resolves memory inconsistency errors of volatile variable when modified by multiple threads. Atomic classes like AtomicInteger, AtomicLong...etc provide synchronized counter in multi threaded environment without using synchronization in code. Atomic classes use CAS mechanism to acheive that, Compare And Set is a lock free approach, it will read the current value and update the modified value only if the current value is not modified. In some cases, if it is already modified, it will redo the same process by reading the updated value. AtomicInteger is equivalent of volatile + synchronization on variables.

**
    When to use: Multiple threads read and modify
**

```
class SynchronizedCounter {
    private AtomicInteger counter = new AtomicInteger(0);

    public void increment() {
        counter.incrementAndGet();
    }

    public int get() {
        counter.get();
    }
}
```

Atomic classes use Compare And Set(CAS) mechanism, the below code explains how this can be done when multiple threads modify and read same data. What it does, tries to store a incremented value if the current value is same as before, if not successful, read the value and do the same again till it gets done.
 

```
int current;
do {
    current = get();
} while(!compareAndSet(current, current+1));

```
