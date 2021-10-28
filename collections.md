# Collections

## Synchronized Collection

A collection represents a group of objects, known as elements. some collection allow duplicate elements and others do not allow duplicate elements, some collection are ordered and others are not ordered. By default, java collection classes such as List, Set are not thread safe means that when multiple threads modifies the collection such ArrayList, HashSet ..etc, it would give inconsistent result, all the classes that implements the collection iterfaces are not thread safe because Synchronized is a method or block level property, It s not a class level property. In order to use the collection classes in a thread safe environment, we need wrap them using Collections.synchronizedCollection(Collection<T>). This method would return thread safe collection which use synchronized monitor to provide thread safety. 

```
List safeList = Collections.synchronizedList(new ArrayList());
Set safeSet = Collections.synchronizedSet(new HashSet<>());
Map safeMap = Collections.synchronizedMap(new HashMap<>());

// synchronization is necessary while iterating a synchronized collection
synchronized(safeList) {
    Iterator itr = safeList.iterator();
    while(itr.hasNext()) {
        System.out.println(itr.next());
    }
}

```

## Concurrent Collection

Concurrent collection in java uses multiple locks to provide concurrency and thread safety whereas synchronized collection uses single lock to acheive thread safety. Concurrent collection such as ConcurrentHashMap which uses multiple locks to acheieve high concurrent reads and updates. CopyOnWriteList, CopyOnWriteSet also part of concurrent collections, instead of using multiple locks, it makes a fresh copy for every mutable operations such add, remove .etc.
Following are some of the list of concurrent collection that are used frequently in multithreaded programming.

ConcurrentHashMap, BlockingQueue (uses multiple locks to improve concurrency)
CopyOnWriteArrayList, CopyOnWriteSet 
ConcurrentLinkedDeque, ConcurrentSkipListMap (uses CAS (Compare and Swap ))


