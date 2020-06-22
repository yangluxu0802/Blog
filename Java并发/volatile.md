### volatile

有序性 ：防止指令重排（编译器Javac JIT 会重排 CPU会重排 内存会重排）happens-before原则（没搞懂）

原子性 ：无法保证原子性，基础的赋值是Java就保证的，像复杂的操作就i++就无法保证

可见性 ：不同线程的缓存数据相互之间不可见，但是volatile采用了硬件底层提供的方式来实现可见性

效率问题：使用volatile修饰变量会降低效率

```java
public class VolatileDemo {

    private static volatile int volatileVal = 0;
    private static int val = 0;
    private static AtomicInteger atomicInteger = new AtomicInteger();
    private static int count = 100000000;

    public static void main(String[] args) {
        new Thread(()->{
            long begin = System.currentTimeMillis();
            for (int i = 0; i < count; i++) {
                VolatileDemo.volatileVal++;
            }
            long end = System.currentTimeMillis();
            System.out.println("volatileVal所需要的时间为" + (end - begin));
        }).start();

        new Thread(()->{
            long begin = System.currentTimeMillis();
            for (int i = 0; i < count; i++) {
                VolatileDemo.val++;
            }
            long end = System.currentTimeMillis();
            System.out.println("val所需要的时间为" + (end - begin));
        }).start();

        new Thread(()->{
            long begin = System.currentTimeMillis();
            for (int i = 0; i < count; i++) {
                VolatileDemo.atomicInteger.incrementAndGet();
            }
            long end = System.currentTimeMillis();
            System.out.println("atomicInteger所需要的时间为" + (end - begin));
        }).start();
    }
}
运行结果：
val所需要的时间为11
atomicInteger所需要的时间为2729
volatileVal所需要的时间为2746
    


    
```

#### 从硬件层面来了解原理

高速缓存：CPU L1 L2 L3缓存，为了弥补处理器与内存处理能力之间的鸿沟

写缓冲器：写缓冲器让处理器在执行写操作时不需要再额外的等待，减少了写操作的延时，提高了处理器的指令执行效率

无效化队列：在引入无效化队列后，处理器在接收到Invalidate消息后，并不马上删除消息中指定地址对应的副本数据，而是将消息存入无效化队列之后就回复Invalidate Acknowledge消息，从而减少了执行写操作的处理器的等待时间。（需要注意的是，有些处理器（如X86）可能并没有使用无效化队列



### 缓存一致性协议-MESI（Modified-Exclusive-Shared-Invalid）

个人理解是系统层面对缓存实现的读写锁



> **M（修改，Modified）：**本地处理器已经修改缓存行，即是脏行，它的内容与内存中的内容不一样，并且此 cache 只有本地一个拷贝(专有)；
>
> **E（专有，Exclusive）：**缓存行内容和内存中的一样，而且其它处理器都没有这行数据；
>
> **S（共享，Shared）：**缓存行内容和内存中的一样, 有可能其它处理器也存在此缓存行的拷贝；

> **I（无效，Invalid）：**缓存行失效, 不能使用。

> **并发读：**
> 当处理器Processor 0要读取缓存中的数据S时，如果发现S所在的缓存条目状态为M、E或S，那么处理器可直接读取数据。
>
> 如果S所在的缓存条目状态状态为 I，说明Processor 0的缓存中不包含S的有效数据。这时，Processor 0会往总线发送一条Read消息来读取S的有效数据，而缓存状态不为 I 的其他处理器（如Process 1）或主内存（其他处理器缓存条目状都为 I 时从主内存读）收到消息后需要回复Read Response，来将有效的S数据返回给发送者。
>
> 需要注意的是，返回有效数据的其他处理器（如Process 1），如果状态为M，则会先将数据写入主内存，此时状态为E，然后在返回Read Response后，再将状态更新为S。
>
> 这样，Processor 0读取的永远是最新的数据，即使其他处理器对这个数据做了更改，也会获取到其他处理器最新的修改信息。
>
> **互斥写:**
> 互斥写
> 当处理器Processor 0要向地址A中写数据时，如果地址A所在的缓存条目状态为E、M，说明Processor 0已拥有该数据的独占权，Processor 0可直接将数据写入A，然后将缓存条目状态改为M
>
> 如果写的缓存条目状态为S，处理器Processor 0需要往总线发送Invalidate消息来获取该缓存条目的独占权，当接收到其他所有处理器返回的Invalidate Acknowledge消息后，Processor 0才会确定自己已获得独占权，然后再将数据更新到地址A中，并将对应的缓存条目状态改为M
>
> 如果写的缓存条目状态为I，处理器Processor 0需要往总线发送Read Invalidate消息来获取该缓存条目的独占权，其他步骤同S
>
> 需要注意的是，如果接收到Invalidate消息的其他其他处理器，缓存条目状态为M，则该处理器会先将数据写入主内存（以方便发送Read Invalidate指令的处理器读到最新值），然后再将状态改为I。
>
> 这样，Processor 0与其他处理器写的时候，永远只有一个处理器能够获得独占权，即实现了互斥写。



参考：

[https://zhuanlan.zhihu.com/p/48844534](https://zhuanlan.zhihu.com/p/48844534)

