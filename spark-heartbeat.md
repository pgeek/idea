spark中Driver-Executor是通过heartbeat进行active的判定的

## Executor端

driver端有一个线程叫做：driver-heartbeater

启动线程的class： **org.apache.spark.executor.Executor**

关键的代码如下：

```
 private val threadPool = ThreadUtils.newDaemonCachedThreadPool("Executor task launch worker")
 
 private def startDriverHeartbeater(): Unit = {
    val intervalMs = conf.getTimeAsMs("spark.executor.heartbeatInterval", "10s")

    // Wait a random interval so the heartbeats don't end up in sync
    val initialDelay = intervalMs + (math.random * intervalMs).asInstanceOf[Int]

    val heartbeatTask = new Runnable() {
      override def run(): Unit = Utils.logUncaughtExceptions(reportHeartBeat())
    }
    heartbeater.scheduleAtFixedRate(heartbeatTask, initialDelay, intervalMs, TimeUnit.MILLISECONDS)
  }
```

**spark.executor.heartbeatInterval** 
控制executor端向driver端发送心跳的频率，单位是秒


## Driver端

driver端有一个接受心跳的线程和kill executor的线程。代码如下：

可以参考的class： **org.apache.spark.HeartbeatReceiver**

```
 // "eventLoopThread" is used to run some pretty fast actions. The actions running in it should not
 // block the thread for a long time.
private val eventLoopThread =
    ThreadUtils.newDaemonSingleThreadScheduledExecutor("heartbeat-receiver-event-loop-thread")

private val killExecutorThread = ThreadUtils.newDaemonSingleThreadExecutor("kill-executor-thread")
```

**spark.rpc.askTimeout** 和 **spark.network.timeout** 表达的含义一样， 可以参考 **org.apache.spark.util.RpcUtils**中的代码。

```
/** Returns the default Spark timeout to use for RPC ask operations. */
  private[spark] def askRpcTimeout(conf: SparkConf): RpcTimeout = {
    RpcTimeout(conf, Seq("spark.rpc.askTimeout", "spark.network.timeout"), "120s")
  }
```