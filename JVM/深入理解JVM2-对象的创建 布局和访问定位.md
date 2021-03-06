# 对象的创建
类是对象的模板，对象是类的实例。  
相信很多人都有这样的社会经验，在工业制造中，很多产品都是通过模具生产出来的。在这个例子中，类就是模具，对象就是产品。那么就有一个疑问，是先有模具还是先有产品？以我们的社会经验来说，肯定是先有模具才能批量制造产品。在Java中也是这样的。  

```
	SingleDog singleDog = new SingleDog();

```

以上代码是个很简单的对象实例化过程，通过这段代码，我们深入Java虚拟机看它是如何执行的？    
  
1：类加载
> 虚拟机遇到一条new指令时，首先将去检查这个指令的参数是否能在常量池中定位到一个类的符号引用，如果存在就检查这个符号引用代表的类是否已被加载、解析和初始化过。如果没有，那必须先执行相应的**类加载过程**     
       
类加载过程对应的就是工业中模具的制造过程。类加载过程是非常重要的，它可以实现很多的功能，例如反射，动态代理等。  

2：分配内存  
> 对象所需的内存大小在类加载过程中就可以确定了（对象大小的确定通过对象在内存中存储布局可以看到），有两种分配方式，  
> 1）“指针碰撞”如果堆内存现在是绝对规整的，所有用过的内存放在一边，没有用过的内存放在另一边，在中间放着一个指针作为分界点的指示器，当要给一个对象分配内存时，就将这个指针向未使用过的那一侧移动相应的与对象大小相等的距离  
> 2）“空闲列表”如果堆内存不是规整的，虚拟机中会维护一张表，表中记录了哪些内存时空闲的，分配的时候从列表上找内存空间就可以了

> 注意：对象创建在虚拟机中是非常频繁的行为，而且就算是仅仅修改一个指针所指向的位置，在并发清空下也并不是线程安全的，也就是说现在正在给A对象分配内存，指针还没来得及修改，B对象也使用了 刚才的指针来分配内存

> 1）我们可以对分配内存空间的动作进行同步处理，实际上虚拟机采用CAS配上失败重试的方式保证更新操作的原子性

> 2）每个线程在java堆中预先分配一小块内存，叫做本地线程分配缓存（TLAB），只有TLAB用完并分配新的TLAB时才需要同步锁定

分配内存对应的就是工业中的物料选择过程。

3：初始化值  
> 内存分配完成后，虚拟机需要将分配到的内存空间都初始化为零值，这样做就能让我们在java代码中不需要给成员变量初始化值就可以获取到零值

4：设置对象头

> 设置如对象的哈希码、对象的GC分代年龄（通过这个计数器来决定是放到新生代还是老年代），元数据信息

初始化值和设置对象头对应的就是工业中产品的生产。  
> 对于虚拟机来说一个新的对象已经创建好了，但是对于java程序来说，这些只是在内存中存储了，<init>方法还没有执行了，执行了之后一个真正可用的对象才算完全产生出来

# 对象的内存布局

产品（对象）已经制造好了，现在去看看产品的具体情况，这就是对象的内存布局。

> 对象在内存中的存储布局可以分为3块区域：对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）。数组与对象类似，只是对象头部分多了数组长度Length的存储长度为4字节。

1：对象头

> Mark Word 与 Class Pointer(类型指针)。  
> Mark Word存储了对象的hashCode、GC信息、锁信息三部分，Class Pointer存储了指向类对象信息的指针。在32位JVM上对象头占用的大小是8字节，64位JVM则是16字节，两种类型的Mark Word 和 Class Pointer各占一半空间大小。  
> 在64位JVM上有一个压缩指针选项-XX:+UseCompressedOops，默认是开启的。开启之后Class Pointer部分就会压缩为4字节，此时对象头大小就会缩小到12字节。  
> 
> 下面是用一张图展示 32位JVM上对象头的内存分布，方便理解。  
> ![](https://pic1.zhimg.com/80/v2-19350088370e7efb480ad5a536b19e54_hd.jpg)

2：对象实际数据
> 对象实际数据包括了对象的所有成员变量，其大小由各个成员变量的大小决定，比如：byte和boolean是1个字节，short和char是2个字节，int和float是4个字节，long和double是8个字节，reference是4个字节（64位系统中是8个字节）。  
> 
> 对于reference类型来说，在32位系统上占用4bytes, 在64位系统上占用8bytes。  
> 
> 这部分的存储顺序会受到虚拟机分配策略参数（FieldsAllocationStyle）和字段在 Java 源码中定义顺序的影响。

3：对象填充数据  
> 对齐填充不是必然存在的，没有特别的含义，它仅起到占位符的作用。
> 
> 由于 HotSpot VM 的自动内存管理系统要求对象起始地址必须是 8 字节的整数倍，也就是说对象的大小必须是 8 字节的整数倍。对象头部分是 8 字节的倍数，所以当对象实例数据部分没有对齐时，就需要通过对齐填充来补全。

通过了解Java对象内存布局，我们发现Java实现锁的关键信息就在对象头中，这为以后我们学习锁打下了基础。

# 对象的访问定位

建立对象是为了使用对象. 
> Java程序 通过 栈上的引用类型数据（reference） 来访问Java堆上的对象

> 由于引用类型数据（reference）在 Java虚拟机中只规定了一个指向对象的引用，但没定义该引用应该通过何种方式去定位、访问堆中的对象的具体位置。所以对象访问方式取决于虚拟机实现。目前主流的对象访问方式有两种：
> **句柄访问**和**直接指针访问**  
> 
> 使用句柄访问方式，java堆将会划分出来一部分内存去来作为句柄池，reference中存储的就是对象的句柄地址。而句柄中则包含对象实例数据的地址和对象类型数据（如对象的类型，实现的接口、方法、父类、field等）的具体地址信息。  
> 如果使用指针访问，那么java堆对象的布局中就必须考虑如何放置访问类型的相关信息（如对象的类型，实现的接口、方法、父类、field等），而reference中存储的就是对象的地址。
> 
> 这两种访问方式各有利弊，使用句柄访最大的好处是reference中存储着稳定的句柄地址，当对象移动之后（垃圾收集时移动对象是非常普遍的行为），只需要改变句柄中的对象实例地址即可，reference不用修改。
>   
> 使用指针访问的好处是访问速度快，它减少了一次指针定位的时间开销，由于java是面向对象的语言，在开发中java对象的访问非常的频繁，因此这类开销积少成多也是非常可观的，反之则提升访问速度。  
> 对于HotSpot虚拟机来说，使用的就是直接指针访问的方式。





