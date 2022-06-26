## Comparing jstack and jcmd with FusionReactor – see how much more you get

In one of our previous [blog](https://www.fusion-reactor.com/blog/java-visualvm-alternatives/) posts we tried to compare Java VisualVM with FusionReactor. Today we will try to compare features that jcmd and jstack provide with FusionReactor.


## jcmd and jstack Definition
### jstack
Let's start with jstack. According to Oracle's documentation
>The jstack command-line utility attaches to the specified process or core file and prints the stack traces of all threads that are attached to the virtual machine, including Java threads and VM internal threads, and optionally native stack frames. The utility also performs deadlock detection.
First, we need to mention that Java VisuaVM provides all the features available by jstack

To make it simple, `jstack` is a terminal based utility that prints the stacktraces of the given JVM process.
For example, in order to see stacktraces of the Java program with **PID** 1302 you need to run the following command in the terminal `jstack 1302`. 
Here is the sample output that you can get from a running Spring-Boot application with embedded Tomcat

```
"http-nio-8080-exec-1" #23 daemon prio=5 os_prio=0 cpu=0.24ms elapsed=12.29s tid=0x00007f153cb0a800 nid=0x156fd waiting on condition  [0x00007f14a60b1000]
   java.lang.Thread.State: WAITING (parking)
        at jdk.internal.misc.Unsafe.park(java.base@11.0.8/Native Method)
        - parking to wait for  <0x000000008e3e0e10> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
        at java.util.concurrent.locks.LockSupport.park(java.base@11.0.8/LockSupport.java:194)
        at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(java.base@11.0.8/AbstractQueuedSynchronizer.java:2081)
        at java.util.concurrent.LinkedBlockingQueue.take(java.base@11.0.8/LinkedBlockingQueue.java:433)
        at org.apache.tomcat.util.threads.TaskQueue.take(TaskQueue.java:146)
        at org.apache.tomcat.util.threads.TaskQueue.take(TaskQueue.java:33)
        at org.apache.tomcat.util.threads.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:1114)
        at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1176)
        at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)
        at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
        at java.lang.Thread.run(java.base@11.0.8/Thread.java:834)
```

As you can see, `jstack` shows us one of the **Tomcat's** thread which is waiting for new requests to come.

jstack is available as a part of the Oracle's JDK since JDK 6. However, we have to mention that since JDK 8 ,Oracle wants to depricate this utility. According to **jstack** [official documentation](https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr016.html)
>The release of JDK 8 introduced Java Mission Control, Java Flight Recorder, and jcmd utility for diagnosing problems with JVM and Java applications. It is suggested to use the latest utility, jcmd instead
Which means that for java versions starting from 8 and higher the `jcmd should be used`
### jcmd
Now let's cover jcmd.
According to [official documentation](https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr006.html)
> The jcmd utility is used to send diagnostic command requests to the JVM, where these requests are useful for controlling Java Flight Recordings, troubleshoot, and diagnose JVM and Java Applications.
`jcmd` is available as part of the JDK starting from version 8. It can do a lot more than `jstack` and is used as a general console based performance tracker tool for JVM. It is used in conjuction with events you are interested in. For example , in order to see the same stacktraces as we got with `jstack` above , the following command must be used `jcmd fusion-reactor.jar Thread.print`. 
From now on , we will compare `jcmd` with FusionReactor because all `jstack` features are already available in `jcmd`


## Comparison

### Console vs Web
The first big difference comes with the way both tools are used. FusionReactor offers a web interface to monitor your JVM process. The interface can be launched locally or through the Cloud in order to monitor remote processes in production. On the other hand `jcmd` can only be used with a terminal. Most people who get used to great user experience from web or desktop apps won't feel comfortable opening the terminal to monitor the performance of the underlying application. Moreover, `jcmd` can only get snapshots of the JVM process that runs in the same host meaning that `jcmd` does not support applications running remotely. All production level backends are running on remote hosts. With `jcmd` the developer will have to have an **ssh** access to the remote machine first





## TODO
1. jcmd only locally
2. ssh access
