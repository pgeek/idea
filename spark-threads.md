# org.apache.spark.ContextCleaner

通过WeakReference的特性，借助于GC，来对资源进行回收、结束处理

清理的对象有：

* RDD
* Accumulator/Accumulable
* Broadcast
* Shuffile

## cleaningThread

线程名字：

```
Spark Context Cleaner
```

消费ReferenceQueue中的reference，进行相应的处理

## periodicGCService


线程的名字：

```
context-cleaner-periodic-gc
```
定时调用System.gc()



