> # 目录结构
### 1. [项目经验](#项目经验)
### 2. [操作系统](#操作系统)
### 3. [计算机网络](#计算机网络)
### 4. [数据结构](#数据结构)
### 5. [数据库](#数据库)
### 6. [设计模式](#设计模式)
### 7. [Java](#Java)
### 8. [java编程基础](#java编程基础)
### 9. [java面向对象](#java面向对象)
### 10. [java集合](#java集合)
### 11. [javaIO](#javaIO)
### 12. [java反射](#java反射)
### 13. [java并发编程](#java并发编程)
### 14. [JVM](#JVM)


> # 正文

## 项目经验

我此刻参与的项目吧。<br>
    我在搜索引擎部分，正在做的搜索引擎的后台管理。前端使用react,后端使用springBoot,数据在elasticsearch中请求。服务器使用node做代理转发请求给springboot。
    业务中，对于搜索关键字及其推荐词、同义词，是可以通过管理平台自定义增改删的，其中热更新是一个难点。我们解决的方式是：开发一个es的插件，由es对aws3进行定时扫描，有新的更新记录，则进行由插件进行读取更新数据到es集群中。 
    我在其中做的是管理后台的自动化测试，以及修改相应测试出的bug。
    做了一个给客户展示搜索界面的demo，前端+后端。
    做了一个快速启动的shell脚本，主要内容是对整个部署在服务器中项目的各部分依次启动。先对插件的打包，并移动到es服务器插件目录下、初始化es数据、启动springboot(docker)、启动代理。

*   可能遇到的问题
    - es 版本为 5.6.2
    - 用的是es提供的java包，请求为 restClient_searchAdmin.performRequest(method,url,Collections.<String,String>emptyMap(),entity)
    - es建表语句
    - es增删改改查
    
## 操作系统  

#### 进程、线程

* 进程：进程是正在运行的程序的实例（an instance of a computer program that is being executed）

    ![进程模型](./images/进程.png)

* 线程

    ![进程模型](./images/线程.png)

所以呢，进程是资源分配的基本单位，而线程是运行与调度的基本单位。

进程有着自己独立的地址空间，是不能共享内存的，而线程之间可以。

线程间有着共享内存，通信很方便。

**进程间通信(IPC，Interprocess communication)**：

1. 管道 
<br>它可以看作是特殊的文件，对于它的读写也可以使用普通的read、write、open等函数，不过它是只存在与内存中的。
    - 普通管道(PIPE):通常有两种限制,一是单工,只能单向传输;二是只能在父子或者兄弟进程间使用.

    - 流管道(s_pipe):去除了第一种限制,为半双工，可以双向传输.

        ![流管道模型](./images/普通管道.png)

    如图,数据流从父进程流向子进程。
    若要数据从子进程流向父进程，则关闭父进程的写端与子进程的读端，打开子进程的写端与父进程的读端，即可。

    - 命名管道(name_pipe):去除了第二种限制,可以在许多并不相关的进程之间进行通讯.
    有路径名与之相关联，它以一种特殊设备文件形式存在于文件系统中.

2. 消息队列

消息队列，是消息的链接表，存放在内核中。一个消息队列由一个标识符（即队列ID）来标识。

    特点：
    1. 消息队列是面向记录的，其中的消息具有特定的格式以及特定的优先级。
    2. 消息队列独立于发送与接收进程。进程终止时，消息队列及其内容并不会被删除。
    3. 消息队列可以实现消息的随机查询,消息不一定要以先进先出的次序读取,也可以按消息的类型读取。

3. 信号量

信号量（semaphore）与已经介绍过的 IPC 结构不同，它是一个计数器。信号量用于实现进程间的互斥与同步，而不是用于存储进程间通信数据。

    特点
    1. 信号量用于进程间同步，若要在进程间传递数据需要结合共享内存。
    2. 信号量基于操作系统的 PV 操作，程序对信号量的操作都是原子操作。
    3. 每次对信号量的 PV 操作不仅限于对信号量值加 1 或减 1，而且可以加减任意正整数。
    4. 支持信号量组。

4. 共用内存

共享内存（Shared Memory），指两个或多个进程共享一个给定的存储区。

    特点
    1. 共享内存是最快的一种 IPC，因为进程是直接对内存进行存取。
    2. 因为多个进程可以同时操作，所以需要进行同步。
    3. 信号量+共享内存通常结合在一起使用，信号量用来同步对共享内存的访问。


5. TCP

通过网络socket与其他进程进行数据交流

#### 例题

有关线程说法正确的是( A C )

A 线程是程序的多个顺序的流动态执行

B 线程有自己独立的地址空间

C 线程不能够独立执行，必须依存在应用程序中,由应用程序提供多个线程执行控制

D 线程是系统进行资源分配和调度的一个独立单位

## 计算机网络

#### osi七层模型、TCP/IP模型

* 应用层    HTTP、SMTP 、telnet、DHCP、PPTP
* 传输层    判断数据包的可靠性，错误重传机制，TCP/UDP协议
    - TCP 协议  基于连接的
* 网络层    路由    ip协议，标识各个网络节点
* 数据链路层  节点到节点数据包的传递,并校验数据包的安全性

![网络模型](./images/网络模型.png)

#### 网络传输
* 不可靠
    - 丢包、重复包
    - 出错
    - 乱序
* 不安全
    - 中间人攻击
    - 窃取
    - 篡改

#### TCP

TCP（Transmission Control Protocol 传输控制协议）是一种面向连接的、可靠的、基于字节流的传输层通信协议

    - 在数据正确性与合法性上，TCP用一个校验和函数来检验数据是否有错误，在发送和接收时都要计算校验和；同时可以使用md5认证对数据进行加密。
    - 在保证可靠性上，采用超时重传和捎带确认机制。
    - 在流量控制上，采用滑动窗口协议，协议中规定，对于窗口内未经确认的分组需要重传。
    - 在拥塞控制上，采用TCP拥塞控制算法（也称AIMD算法）。该算法主要包括三个主要部分：1）加性增、乘性减；2）慢启动；3）对超时事件做出反应。


    * TCP 是面向连接的传输层协议
    * 每一条 TCP 连接只能有两个端点(endpoint),每一条 TCP 连接只能是点对点的（一对一）
    * TCP 提供可靠交付的服务
    * TCP 提供全双工通信
    * 面向字节流

* 报头

    ![TCP报文头模型](./images/TCP报文头模型.png)

    我们来分析分析每部分的含义和作用

    * 源端口号/目的端口号: 各占两个字节，端口是传输层与应用层的服务接口
    * 32位序号: 占4字节，连接中传送的数据流中的每一个字节都编上一个序号。序号字段的值则指的是本报文段所发送的数据的第一个字节的序号。
    * 确认序号: 占 4 字节,是期望收到对方的下一个报文段的数据的第一个字节的序号
    * 4位首部长度: 表示该tcp报头有多少个4字节(32个bit)
    * 6位保留:  占 6 位,保留为今后使用,但目前应置为 0
    * 6位标志位：

        URG: 标识紧急指针是否有效 

        ACK: 标识确认序号是否有效 。只有当 ACK=1 时确认号字段才有效.当 ACK=0 时,确认号无效

        PSH: 用来提示接收端应用程序立刻将数据从tcp缓冲区读走 

        RST: 要求重新建立连接. 我们把含有RST标识的报文称为复位报文段 

        SYN: 请求建立连接. 我们把含有SYN标识的报文称为同步报文段 。　同步 SYN = 1 表示这是一个连接请求或连接接受报文

        FIN: 通知对端, 本端即将关闭. 我们把含有FIN标识的报文称为结束报文段。FIN=1 表明此报文段的发送端的数据已发送完毕,并要求释放运输连接

    * 16位窗口大小: 
    * 16位检验和: 由发送端填充, 检验形式有CRC校验等. 如果接收端校验不通过, 则认为数据有问题. 此处的校验和不光包含TCP首部, 也包含TCP数据部分. 
    * 16位紧急指针: 用来标识哪部分数据是紧急数据.
    选项和数据暂时忽略

* 三次握手

    客户端CLOSE状态，服务器LISTEN(监听)状态<br>
    客户端向服务器主动发出连接请求, 服务器被动接受连接请求

    客户端 -- > 服务端 发送SYN包，请求建立连接<br>
    服务端 -- > 客户端 发送SYN+ACK包，确认收到连接请求，并回复响应<br>
    客户端 -- > 服务端 发送ACK包，确认回复，建立连接

    ![TCP三次握手模型](./images/TCP三次握手模型.gif)

    **为什么不用两次?**

        主要是为了防止已经失效的连接请求报文突然又传送到了服务器，从而产生错误。

        如果使用的是两次握手建立连接，那么假设：
            客户端发送的第一个请求在网络中滞留较长时间，由于客户端迟迟没有收到服务端的回应，重新发送请求报文，服务器收到后与客户端建立连接，传输数据，然后关闭连接。此时滞留的那一次请求因为网络通畅了，到达了服务器，这个报文本应是失效的，但是两次握手的机制将会让客户端与服务端再次建立连接，导致不必要的错误和资源浪费。

            如果采用的是三次握手，就算失效的报文传送过来，服务端就收到报文并回复了确认报文，但是客户端不会再次发送确认。由于服务端没有收到确认，就知道客户端没有连接。

    **为什么不用四次？**

        因为三次就足够了，四次就多余了。

* 四次挥手

    客户端和服务器都是处于ESTABLISHED状态<br>
    客户端主动断开连接，服务器被动断开连接

    客户端 -- > 服务端 发送FIN=1(连接释放报文)，并且停止发送数据，进入FIN-WAIT-1(终止等待1)状态<br>
    服务端 -- > 客户端 发送确认报文ACK，并带上自己的序列号seq=v，进入了CLOSE-WAIT(关闭等待)状态<br>
    客户端 收到服务器确认，进入 FIN-WAIT-2(终止等待2)状态，等待服务器发送连接释放报文（在这之前还需要接受服务器发送的最终数据）<br>
    服务端 -- > 向客户端发送连接释放报文，FIN=1，确认序号为v+1,服务器就进入了LAST-ACK（最后确认）状态，等待客户端的确认。<br>
    客户端 -- > 服务端 发送ACK包，确认,断开连接<br>
    服务器 收到确认,断开连接

    ![TCP四次挥手模型](./images/TCP四次挥手模型.gif)

    **为什么最后客户端还要等待 2*MSL的时间呢?**

        MSL（Maximum Segment Lifetime），TCP允许不同的实现可以设置不同的MSL值

        1. 保证客户端发送的最后一个ACK报文能够到达服务器，如果ACK报文丢失，服务器未收到确认，会再次发送，而客户端就能在这个2MSL时间段内收到重传的报文，接着给出回应报文，并且会重启2MSL计时器。

        2. 防止类似与"三次握手"中提到了的"已经失效的连接请求报文段"出现在本连接中。客户端发送完最后一个确认报文后，在这个2MSL时间中，就可以使本连接持续的时间内所产生的所有报文段都从网络中消失。这样新的连接中不会出现旧连接的请求报文。(简单讲，就是这段时间所有的报文段会慢慢消失，以至于其余的连接请求是不会有正确确认的)

    **为什么建立连接是三次握手，关闭连接确是四次挥手呢？**

        断开连接时，服务器收到对方的FIN包后，仅仅表示对方不在发送数据了，但是还能接收数据。所以己方还可能有数据发送给客户端，因此己方ACK与FIN包会分开发送，从而导致多了一次。

    **如果已经建立了连接, 但是客户端突发故障了怎么办?**

        TCP设有一个保活计时器。
        显然，如果客户端发生故障，服务器不能一直等下去，浪费资源。
        服务器每收到一次客户端的请求都会重置复位这个计时器，时间通常设置为2小时。两小时没有收到客户端请求，会发送一个探测报文段，以后每隔75分钟发送一次，连续10次后，客户端仍没反应，则认为客户端故障，关闭连接。

* 确认应答机制(ACK机制)

    TCP将每个字节的数据都进行了编号, 即为序列号

    ![序列号模型](./images/序列号模型.png)

    每一个ACK都带有对应的确认序列号, 意思是告诉发送者, 我已经收到了哪些数据; 下一次你要从哪里开始发. 

    比如, 客户端向服务器发送了1005字节的数据, 服务器返回给客户端的确认序号是1003, 那么说明服务器只收到了1-1002的数据. 
    1003, 1004, 1005都没收到. 
    此时客户端就会从1003开始重发.

* 超时重传机制

    特定的时间间隔内，发生网络丢包(到客户端的数据包丢失或者客户端确认的ACK包丢失)，主机未收到另一端的确认应答，就会进行重发。

    这种情况下, 主机B会收到很多重复数据. 
    那么TCP协议需要识别出哪些包是重复的, 并且把重复的丢弃. 
    这时候利用前面提到的序列号, 就可以很容易做到去重.

    **超时时间如何确定?**

    TCP为了保证任何环境下都能保持较高性能的通信, 因此会动态计算这个最大超时时间.

* 滑动窗口

* 流量控制

* 拥塞控制

* 延迟应答


## 数据结构

#### 搜索二叉树 

#### 平衡二叉树

#### 红黑树

AVL是高度平衡的树，插入删除都需要大量的左旋右旋进行重新平衡
而红黑树放弃了极致平衡以追求插入删除时的效率

红黑树的特点：
1.  节点非黑即红
2.  叶子节点为黑色
3.  到叶子节点的任意路径下的黑色节点个数一致
4.  红色节点的两个子节点都为黑色

#### b树

#### 用两个栈实现一个队列或者俩个队列实现一个栈

1. 区别与联系

相同点：
（1）栈和队列都是控制访问点的线性表；
（2）栈和队列都是允许在端点处进行数据的插入和删除的数据结构；

不同点：
（1）栈遵循“后进先出（LIFO）”的原则，即只能在该线性表的一头进行数据的插入和删除，该位置称为“栈顶”，而另外一头称为“栈底”；根据该特性，实现栈时用顺序表比较好；
（2）队列遵循“先进先出（FIFO）”的原则，即只能在队列的尾部插入元素，头部删除元素。根据该特性，在实现队列时用链表比较好

2. 两站实现队列
栈是后进先出的，所以两个栈 一个栈用来入栈，而将栈中的数据再次出栈存入另一个栈中，这样经过两次后进先出， 则实现了先进先出

3. 两队列实现栈
始终保持一个队列为空，需要出队时，队列抛出n-1个数据到另一队列中，只留下一个top1数据，这样那个队列再次pop,pop的数据就是队首元素了。

## 数据库

#### 范式
*   1NF 一个属性不允许在分成多个属性，即每个属性具有 **原子性**
*   2NF **完全函数依赖**，第二范式的目标就是消除函数依赖关系中左边存在的冗余属性。例如，（学号，班级）->姓名，事实上，只需要学号就能决定姓名，因此班级是冗余的，应该去掉。
*   3NF **消除传递依赖**  数据库中的属性依赖仅能依赖于主属性，不存在于其他非主属性的关联。
*   BCNF
    *   所有非主属性对每一个码都是完全函数依赖
    *   所有的主属性对于每一个不包含它的码，也是完全函数依赖
    *   没有任何属性完全函数依赖于非码的任意一个组合。
    *   R属于3NF，不一定属于BCNF，如果R属于BCNF，一定属于3NF。
    *   例：配件管理关系模式WPE（WNO，PNO，ENO，QNT）分别表仓库号，配件号，职工号，数量。有以下条件
        a.一个仓库有多个职工。
        b.一个职工仅在一个仓库工作。
        c.每个仓库里一种型号的配件由专人负责，但一个人可以管理几种配件。
        d.同一种型号的配件可以分放在几个仓库中。

#### 特性 
*   A atomicity 原子性  
整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
*   C consistency 一致性 
一个事务可以封装状态改变（除非它是一个只读的）。事务必须始终保持系统处于一致的状态，不管在任何给定的时间并发事务有多少。
*   I isolation 隔离性 
隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相同的时间内，执行相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请求，使得在同一时间仅有一个请求用于同一数据。
*   D durability 持久性
在事务完成以后，该事务对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

#### 隔离级别
事务的隔离级别有四种，隔离级别高的数据库的可靠性高，但并发量低，而隔离级别低的数据库可靠性低，但并发量高，系统开销小

1. READ UNCOMMITED 未提交读 **更新的瞬间加 行级共享锁**
事务还没提交，而别的事务可以看到他其中修改的数据的后果，-->脏读(该事物出错而回滚，其他事物已读到改变的值，发生脏读)。
2. READ COMMITED 提交读  **对被读取的数据加 行级共享锁 读完释放** **更新的瞬间加 行级排他锁**
其他事务只能看到已经完成的事务的结果，正在执行的，是无法被其他事务看到的。这种级别会出现读取旧数据的现象。大多数数据库系统的默认隔离级别是READ COMMITTED
也容易发生丢失更新： 如果A、B同时获取资源X，然后事务A先发起更新记录X，那么事务B将等待事务A完成，然后获得记录X的排他锁，进行更改，这样事务A的更新就会丢失。

    事务A-------------------------------------------事务B <br>
    读取X=100（同时上共享锁）-----------------读取X=100（同时上共享锁） <br>
    读取成功（释放共享锁）----------------------读取成功（释放共享锁）<br>
    UPDATE X=X+100 （上排他锁）--------------	 <br>
    --------------------------------------------------UPDATING A（等待事务A释放对X的排他锁）<br>
    事务成功（释放排他锁）X=200 -------------	 <br>
    --------------------------------------------------UPDATE X=X+200（成功上排他锁）
                                    事务成功（释放排他锁）X=300<br>
3. REPEATBLE COMMITED 可重复读 **对被读取的数据加 行级共享锁 事务结束释放** **更新的瞬间加 行级排他锁**
事务结束才释放行级共享锁 ，这样保证了可重复读（既是其他的事务职能读取该数据，但是不能更新该数据）。　会导致幻读
4. SERIALIZABLE 可串行化
SERIALIZABLE是最高的隔离级别，它通过强制事务串行执行（注意是串行），避免了前面的幻读情况，由于他大量加上锁(加表锁)，导致大量的请求超时，因此性能会比较底下，再特别需要数据一致性且并发量不需要那么大的时候才可能考虑这个隔离级别

#### 锁机制
*   共享锁 (S锁)
由读表操作加上的锁，加锁后其他用户只能获取该表或行的共享锁，不能获取排它锁，也就是说只能读不能写
*   排它锁 (X锁)
由写表操作加上的锁，加锁后其他用户不能获取该表或行的任何锁，典型是mysql事务</br>

根据锁的范围，可以分为
*   表锁
给整张表加锁
*   行锁
给行数据加锁

因此锁可以分为表级共享锁、行级共享锁、表级排它锁、行级排它锁。

#### 期间造的问题
*   丢失更新  
*   脏读（未提交读） 
*   不可重复读   一个事务在自己没有更新数据库数据的情况，同一个查询操作执行两次或多次的结果应该是一致的；如果不一致，就说明为不可重复读。
*   幻读  事务A读的时候读出了15条记录，事务B在事务A执行的过程中 增加 了1条，事务A再读的时候就变成了 16 条，这种情况就叫做幻影读。


## 设计模式

> 源：https://www.cnblogs.com/geek6/p/3951677.html

#### 设计模式的六大原则

* 总原则：开闭原则（Open Close Principle）

    开闭原则就是说对扩展开放，对修改关闭。
    
    在程序需要进行拓展的时候，不能去修改原有的代码，而是要扩展原有代码，实现一个热插拔的效果。

1. 单一职责原则

    不要存在多于一个导致类变更的原因，也就是说每个类应该实现单一的职责，如若不然，就应该把类拆分。

2. 里氏替换原则（Liskov Substitution Principle）

    任何基类可以出现的地方，子类一定可以出现。

    在继承关系中，父类的对象如果替换为子类的对象，他原来执行的行为依然保持不变。这样来说，java的多态正是依赖于里氏替换原则的。

3. 依赖倒转原则（Dependence Inversion Principle）

    面向接口编程，依赖于抽象而不依赖于具体。写代码时用到具体类时，不与具体类交互，而与具体类的上层接口交互。

4. 接口隔离原则（Interface Segregation Principle）

    每个接口中不存在子类用不到却必须实现的方法，如果不然，就要将接口拆分。使用多个隔离的接口，比使用单个接口（多个接口方法集合到一个的接口）要好。

5. 迪米特法则（最少知道原则）（Demeter Principle）

    一个类对自己依赖的类知道的越少越好。也就是说无论被依赖的类多么复杂，都应该将逻辑封装在方法的内部，通过public方法提供给外部。这样当被依赖的类变化时，才能最小的影响该类。

6. 合成复用原则（Composite Reuse Principle）

    原则是尽量首先使用合成/聚合的方式，而不是使用继承。

#### 简单工厂模式

就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建

![简单工厂模型](./images/简单工厂模型.png)

```
public interface Sender {  
    public void Send();  
}  

public class MailSender implements Sender {  
    @Override  
    public void Send() {  
        System.out.println("this is mailsender!");  
    }  
}  

public class SmsSender implements Sender {  
  
    @Override  
    public void Send() {  
        System.out.println("this is sms sender!");  
    }  
}  

//1. 依据传入的字符串创建对象
//如果传入的字符串有误，不能正确创建对象
public class SendFactory {  
    public Sender produce(String type) {  
        if ("mail".equals(type)) {  
            return new MailSender();  
        } else if ("sms".equals(type)) {  
            return new SmsSender();  
        } else {  
            System.out.println("请输入正确的类型!");  
            return null;  
        }  
    }  
}  

//2. 多个工厂方法分别创建不同的对象
public class SendFactory {  
    public Sender produceMail(){  
        return new MailSender();  
    }  
    public Sender produceSms(){  
        return new SmsSender();  
    }
}

//3. 多个静态方法分别创建不同的对象
//相较于上一种，不需要实例化工厂类
public class SendFactory {  
    public static Sender produceMail(){  
        return new MailSender();  
    }  
    public static Sender produceSms(){  
        return new SmsSender();  
    }
}
```
总体来说，工厂模式适合：凡是出现了大量的产品需要创建，并且具有共同的接口时，可以通过工厂方法模式进行创建。

#### 工厂方法模式（Factory Method）

简单工厂模式有一个问题就是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则.

所以，用到工厂方法模式，创建一个工厂接口和创建多个工厂实现类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码。

![工厂方法模式](./images/工厂方法模型.png)

```
public interface Sender {  
    public void Send();  
} 

public class MailSender implements Sender {  
    @Override  
    public void Send() {  
        System.out.println("this is mailsender!");  
    }  
}  

public class SmsSender implements Sender {  
    @Override  
    public void Send() {  
        System.out.println("this is sms sender!");  
    }  
}  

// 工厂接口
public interface Provider {  
    public Sender produce();  
}  

public class SendMailFactory implements Provider {  
    @Override  
    public Sender produce(){  
        return new MailSender();  
    }  
} 

public class SendSmsFactory implements Provider{  
  
    @Override  
    public Sender produce() {  
        return new SmsSender();  
    }  
}   

// 调用时

public class Main {  
  
    public static void main(String[] args) {  
        Provider provider = new SendMailFactory();  
        Sender sender = provider.produce();  
        sender.Send();  
    }  
}  

```

对于增加改变的产品，只需实现Provider接口，做一个相应的工厂类就行了。拓展性较好。

#### 抽象工厂模式

(*-*) 等等哈，没抽象明白！ 

#### 单例模式 
*   适用场景：
    1. 有些类对象的创建与销毁非常耗费资源，而且有经常用到
    2. 需要生成唯一序列话的环境，如部分窗口对象，关闭后打开我们需要保持在之前的界面，
    3. 方便资源互相通信的环境， 如一个log日志单例类，不同地方的的调用可以将日志按顺序记录到一个文件中。
*   优点：
    1. 实现了对唯一实例的访问的可控
    2. 对于一些频繁创建和销毁的对象来说可以提高系统的性能
*   缺点：
    1. 滥用单例会带来一些负面问题，如为了节省资源将数据库连接池对象设计为单例类，可能会导致共享连接池对象的程序太多，而出现连接池溢出
    2. 如果实例化的对象长时间不被利用，系统会认为该对象是垃圾而被回收，这可能会导致对象状态的丢失
*   实现
    1. 饿汉式 初始化时直接创建一个单例对象 初始启动时创建对象延迟启动，而且对象长时间不用占用一定空间，浪费<br>
    2. 懒汉式 需要的时候再去创建。
      *  线程不安全
      *  线程安全 
            *  synchronized锁住方法
            *  synchronized锁部分代码块，缩小锁粒度
            *  内部类，利用类的加载一定线程安全的原因

饿汉式

    ```
    static instance = new SingleInstance();
    ```

懒汉式(线程不安全)

    ```
    static instance = null;
    ...
    getInstance(){
        if(instance==null)
            return new SingleInstance();
    }
    ```

懒汉式加锁synchronized(低效)

    ```
    synchronized getInsance(){
        if (instance==null){
            return new SingleInstance();
        }
    }
    ```

懒汉式双重锁检查(volatile防止指令重排)

    ```
    static volatile instance = null;
    ...
    getInstance(){
        if(instance==null){
            synchronized(SingleInstance.class){
                if (instance==null){
                    return new SingleInstance();
                }
            }
        }
    } 
    ```

内部类(在方法getInstance()调用时加载内部类，利用classLoader保证了同步)

    ```
    // 内部类
    private static class InnerSingleInstance {
        private staic final SingleInstance INTANCE = new SingleInstance();
    }
    private SingleInstance(){};
    public static SingleInstance getInstance(){
        return InnerSingleInstance.INSTANCE;
    }
    ```

#### 代理模式

*   1. 实现类与代理类 都继承同一个接口， 代理类的方法实现实际是调用实现类的该方法实现。 如：spring AOP中的jdk动态代理

    ![代理模式1](./images/代理模式1.png)


*   2. 代理类继承实现类，覆盖其中的方法。 如：spring AOP 中的cglib代理

    ![代理模式2](./images/代理模式2.png)

JDK动态代理只能对实现了接口的类生成代理对象；

cglib可以对任意类生成代理对象，它的原理是对目标对象进行继承代理，如果目标对象被final修饰，那么该类无法被cglib代理。

#### 适配器模式


#### 职责链模式

将能够处理同一类请求的对象连成一条链，使这些对象都有机会处理请求，所提交的请求沿着链传递，从而避免请求的发送者和接受者之间的耦合关系。链上的对象逐个判断是否有能力处理该请求，如果能则处理，如果不能，则传递给链上的下一个对象。直到有一个对象处理它为止。

场景：
    打牌时，轮流出牌
    接力赛跑
    请假审批
    公文审批

client ---------->       Handler( 接口 setSuccessor/HandleRequest)   -------------------concreteHAndler1  ------------------ concreteHandler2 ...

## java编程基础

##### String类为什么不可变

```
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence
{
    /** String本质是个char数组. 而且用final关键字修饰.*/
    private final char value[];
```

1. 字符串常量池的需要

字符串常量池是Java内存中一个特殊的存储区域，当创建一个String对象时，假如此字符串值在常量池中已经存在，则不会创建一个新的对象，而是引用已存在的对象。
因为String是不可变类，可靠。如果String可变，则某个对象的改变会引起其他对象的改变。

2. 允许String对象缓存HashCode

java中String对象的哈希码被频繁的使用，比如hashMap等容器中。
字符串不变保证了hash码的唯一性。如果可变，string改变后将可能导致出现相同的hashCode

3. 安全性

String类被许多类库作为参数，例如，网络连接地址url，文件路径path，还有反射机制所需要的String参数等，假若String不是固定不变的，将会引起各种安全隐患

#### java中Byte怎么转换为String

* String转化为byte[]数组

```
String str = "abcd";
byte[] bs = str.getBytes();
```

* byte[]数组转化为String字符串

```
byte[] bs1 = {97,98,100};
String s = new String(bs1);
```

* 设置格式

```
byte[] srtbyte = {97,98,98};
String res = new String(srtbyte,"UTF-8");
```

## java面向对象

## java集合

## javaIO

## java反射

## java并发编程

#### 三个并发编程概念

*   原子性问题
    * 原子是世界上的最小单位，具有不可分割性
    * 一个操作是原子操作，那么我们称它具有原子性。

即一个操作或者多个操作 要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行。

*   可见性问题

指当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。

*   有序性问题

即程序执行的顺序按照代码的先后顺序执行。


#### Synchronized与ReenTrantLock

两者较大的区别是，synchronized是JVM层面的锁；而ReenTrantLock是jdk提供的API层面的互斥锁，需要lock()和unlock()方法配合try/finally语句块来完成。

1. Synchronized

    Synchronized通过编译，会在同步块的前后分别形成monitorenter和monitorexit这两个字节码指令。执行monitorenter指令时，首先要尝试获取对象锁。如果这个对象没被锁定，或者当前线程已经拥有了那个对象锁，把锁的计算器加一，相应的，在执行monitorexit指令时会将锁计算器减一，当计算器为零时，所就释放了。


    每个对象有一个监视器锁（monitor）。当monitor被占用时就会处于锁定状态，线程执行monitorenter指令时尝试获取monitor的所有权，过程如下：
    *    1. 如果monitor的进入数为0，则该线程进入monitor，然后将进入数设置为1，该线程即为monitor的所有者。
    *    2. 如果线程已经占有该monitor，只是重新进入，则进入monitor的进入数加1.
    *    3. ’如果其他线程已经占用了monitor，则该线程进入阻塞状态，直到monitor的进入数为0，再重新尝试获取monitor的所有权。

    **synchronized锁住的是代码还是对象？**

        synchronized锁住的是对象，同一个对象中的方法在此访问时，并不会申请锁，而是计数加一，所以synchronized是可重入锁。
        


2. ReentrantLock

    ReentrantLock是java.util.concurrent包下提供的一套互斥锁，相比Synchronized，ReentrantLock类提供了一些高级功能，主要有以下3项：
    *    1. 等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized来说可以避免出现死锁的情况。
    *    2. 公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized锁非公平锁，ReentrantLock默认的构造函数是创建的非公平锁，可以通过参数true设为公平锁，但公平锁表现的性能不是很好。
    *    3. 锁绑定多个条件，一个ReentrantLock对象可以同时绑定对个对象。
    *    4. 常见api
        <br>isFair()        //判断锁是否是公平锁
    　　 <br>isLocked()    //判断锁是否被任何线程获取了 
    　　 <br>isHeldByCurrentThread()   //判断锁是否被当前线程获取了 
    　　 <br>hasQueuedThreads()   //判断是否有线程在等待该锁 
 

    ```
    private Lock lock=new ReentrantLock();
    public void run() {
        lock.lock();
        try{
            for(int i=0;i<5;i++)
                System.out.println(Thread.currentThread().getName()+":"+i);
        }finally{
            lock.unlock();
        }
    }
    ```

#### 缓存一致性

解决缓存不一致的问题：

1. 通过在总线加LOCK#锁的方式

    因为CPU和其他部件进行通信都是通过总线来进行的，如果对总线加LOCK#锁的话，也就是说阻塞了其他CPU对其他部件访问（如内存），从而使得只能有一个CPU能使用这个变量的内存。

2. 通过缓存一致性协议

    最出名的就是Intel 的MESI协议，MESI协议保证了每个缓存中使用的共享变量的副本是一致的。它核心的思想是：当CPU写数据时，如果发现操作的变量是共享变量，即在其他CPU中也存在该变量的副本，会发出信号通知其他CPU将该变量的缓存行置为无效状态，因此当其他CPU需要读取这个变量时，发现自己缓存中缓存该变量的缓存行是无效的，那么它就会从内存重新读取。

#### volatile 关键字

1. 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。
2. 禁止进行指令重排序。

但是并不能保证 **原子性**,即：i用volatile修饰,1000个线程调i++,结果并不一定1000.因为自增操作不具备原子性，它包括读取变量的原始值、进行加一操作、写入内存。那么就是说自增操作的三个子操作可能会分割执行。

#### Java原子类

在java 1.5的java.util.concurrent.atomic包下提供了一些原子操作类，即对基本数据类型的 自增（加1操作），自减（减1操作）、以及加法操作（加一个数），减法操作（减一个数）进行了封装，保证这些操作是原子性操作。atomic是利用CAS来实现原子性操作的（Compare And Swap），CAS实际上是利用处理器提供的CMPXCHG指令实现的，而处理器执行CMPXCHG指令是一个原子性操作。

    AtomicBoolean 
    AtomicInteger 
    AtomicIntegerArray 
    AtomicIntegerFieldUpdater 
    AtomicLong 
    AtomicLongArray 
    AtomicLongFieldUpdater 
    AtomicMarkableReference 
    AtomicReference 
    AtomicReferenceArray 
    AtomicReferenceFieldUpdater 
    AtomicStampedReference 
    DoubleAccumulator 
    DoubleAdder 
    LongAccumulator 
    LongAdder


- 实现原理

    ```
    /**
        * Atomically increments by one the current value.
        *
        * @return the updated value
        */
        public final int incrementAndGet() {
            for (;;) {
                int current = get();
                int next = current + 1;
                if (compareAndSet(current, next))
                    return next;
            }
        }
    ```
1. 先获取当前的value值 
2. 对value加一 
3. 第三步是关键步骤，调用compareAndSet方法来来进行原子更新操作，这个方法的语义是：

    先检查当前value是否等于current，如果相等，则意味着value没被其他线程修改过，更新并返回true。如果不相等，compareAndSet则会返回false，然后循环继续尝试更新。


#### CAS(Compare-and-Swap)　

CAS算法是由硬件直接支持来保证原子性的，有三个操作数：内存位置V、旧的预期值A和新值B，当且仅当V符合预期值A时，CAS用新值B原子化地更新V的值，否则，它什么都不做。

- CAS的ABA问题

    当然CAS也并不完美，它存在"ABA"问题，假若一个变量初次读取是A，在compare阶段依然是A，但其实可能在此过程中，它先被改为B，再被改回A，而CAS是无法意识到这个问题的。CAS只关注了比较前后的值是否改变，而无法清楚在此过程中变量的变更明细，这就是所谓的ABA漏洞。 

#### 锁相关概念

- 可重入锁

    如果锁具备可重入性，则称作为可重入锁。像synchronized和ReentrantLock都是可重入锁，可重入性在我看来实际上表明了锁的分配机制：基于线程的分配，而不是基于方法调用的分配。举个简单的例子，当一个线程执行到某个synchronized方法时，比如说method1，而在method1中会调用另外一个synchronized方法method2，此时线程不必重新去申请锁，而是可以直接执行方法method2。 

    ```
    public MyClass{
        public synchronized void method1(){
            method2();
        }
        public synchronized void method2(){

        }
    }
    ```

    机制：每个锁都关联一个请求计数器和一个占有他的线程，当请求计数器为0时，这个锁可以被认为是unhled的，当一个线程请求一个unheld的锁时，JVM记录锁的拥有者，并把锁的请求计数加1，如果同一个线程再次请求这个锁时，请求计数器就会增加，当该线程退出syncronized块时，计数器减1，当计数器为0时，锁被释放。

- 可中断锁

    可中断锁：顾名思义，就是可以相应中断的锁。 

    在Java中，synchronized就不是可中断锁，而Lock是可中断锁。 
    
    如果某一线程A正在执行锁中的代码，另一线程B正在等待获取该锁，可能由于等待时间过长，线程B不想等待了，想先处理其他事情，我们可以让它中断自己或者在别的线程中中断它，这种就是可中断锁。 

- 公平锁

    公平锁即尽量以请求锁的顺序来获取锁。比如同是有多个线程在等待一个锁，当这个锁被释放时，等待时间最久的线程（最先请求的线程）会获得该所，这种就是公平锁。
     
    非公平锁即无法保证锁的获取是按照请求锁的顺序进行的。这样就可能导致某个或者一些线程永远获取不到锁。 
    
    在Java中，synchronized就是非公平锁，它无法保证等待的线程获取锁的顺序。

    而对于ReentrantLock和ReentrantReadWriteLock，它默认情况下是非公平锁，但是可以设置为公平锁。

    ```
    ReentrantLock lock = new ReentrantLock(true); \\公平锁
    ```
 

- 读写锁

    读写锁将对一个资源（比如文件）的访问分成了2个锁，一个读锁和一个写锁。 
    
    正因为有了读写锁，才使得多个线程之间的读操作不会发生冲突。 
    
    ReadWriteLock就是读写锁，它是一个接口，ReentrantReadWriteLock实现了这个接口。 
    
    可以通过readLock()获取读锁，通过writeLock()获取写锁
 
- 偏向锁

    java偏向锁(Biased Locking)是java6引入的一项多线程优化.它通过消除资源无竞争q情况下的同步原语,进一步提高了程序的运行性能.

    偏向锁，顾名思义，它会偏向于第一个访问锁的线程，如果在接下来的运行过程中，该锁没有被其他的线程访问，则持有偏向锁的线程将永远不需要触发同步。 

    如果在运行过程中，遇到了其他线程抢占锁，则持有偏向锁的线程会被挂起，JVM会尝试消除它身上的偏向锁，将锁恢复到标准的轻量级锁。(偏向锁只能在单线程下起作用) 

    因此 流程是这样的 偏向锁->轻量级锁->重量级锁

    偏向锁，简单的讲，就是在锁对象的对象头中有个ThreaddId字段，这个字段如果是空的，第一次获取锁的时候，就将自身的ThreadId写入到锁的ThreadId字段内，将锁头内的是否偏向锁的状态位置1.这样下次获取锁的时候，直接检查ThreadId是否和自身线程Id一致，如果一致，则认为当前线程已经获取了锁，因此不需再次获取锁，略过了轻量级锁和重量级锁的加锁阶段。提高了效率。 
     
 
- 乐观锁，悲观锁

#### 锁的优化策略



#### 死锁、饥饿
* 死锁(deadlock)

指的是两个或者两个以上的进程相互竞争系统资源，导致进程永久阻塞。

* 饥饿(starvation)

指的是等待时间已经影响到进程运行，此时成为饥饿现象。如果等待时间过长，导致进程使命已经没有意义时，称之为“饿死”。


#### 生产者消费者模型

*   问题的核心是：
    1.要保证不让生产者在缓存还是满的时候仍然要向内写数据;
    2.不让消费者试图从空的缓存中取出数据。

多个生产者进行生产，多个消费者进行消费，缓冲区作为双方的纽带(阻塞队列)
当生产者生产较多的产品，超过缓冲区的容量时，阻塞生产者进程
当缓冲区中无产品时，消费者进行阻塞

#### 进程间通信

#### java线程池

java.util.concurrent.Executors工厂类可以创建四种类型的线程池，通过Executors.new***方式创建
分别为newFiexdThreadPool、newCachedThreadPool、newSingleThreadPool、ScheduledThreadPool

*   newFixedThreadPool 

    ```
    public static ExecutorService newFixedThreadPool(int nThreads){
        return new ThreadPoolExecutor(nThreads, nThreads, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>());
    }
    ```

    创建容量固定的线程池
    阻塞队列采用LinkedBlockingQueue (poll()、take()),它是一种无界队列
    由于阻塞队列是一个无界队列，因此永远不可能拒绝执行任务
    由于采用无界队列，实际线程数将永远维持在nThreads,因此maximumPoolSize和KeepAliveTime将无效

*   newCachedThreadPool

    ```
    public static ExecutorService newCachedThreadPool(){
        return new ThreadPoolExecutor(0, Interger.MAX_VALUE, 60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
    }
    ```

    CachedThreadPool是一种可以无限扩容的线程池
    CachedThreadPool比较适合执行时间片较小的任务
    KeepAliveTime为60，意味着线程空闲时间超过60秒就会被杀死
    阻塞队列采用SynchronousQueue,这种阻塞队列没有存储空间，意味着只要有任务到来，就必须得有一个工作线程进行处理，如果当前没有空闲线程，就新建一个线程

*   SingleThreadExecutor

    ```
    public static ExecutorService newSingleExecutor(){
        return new ThreadPoolExecutor(1, 1, 0L, TimeUnit.MILLSECONDS, new LinkedBlockingQueue<Runnable>());
    }
    ```

    SingleThreadExecutor 只会创建一个工作线程来处理任务

*   ScheduledThreadPool接收ScheduleFutureTask类型的任务，提交任务的方式有两种
    1. scheduledAtFixedRate； 
    2. scheduledWithFixedDelay； 
    - SchduledFutureTask接收参数： 
        time：任务开始时间 
        sequenceNumber：任务序号 
        period：任务执行的时间间隔 
    - 阻塞队列采用DelayQueue，它是一种无界队列； 
    - DelayQueue内部封装了一个PriorityQueue，它会根据time的先后排序，若time相同，则根据sequenceNumber排序； 
    - 工作线程执行流程： 
        1. 工作线程会从DelayQueue中取出已经到期的任务去执行； 
        2. 执行结束后重新设置任务的到期时间，再次放回DelayQueue。

## JVM

## oom

* 内存溢出 out of memory

程序在申请内存空间时，没有足够的内存空间供其使用，就发生了溢出。如申请一个

* 内存泄漏 memory leak

是指程序在申请内存后，无法释放已申请的内存空间，一次内存泄露危害可以忽略，但内存泄露堆积后果很严重，无论多少内存,迟早会被占光。

*   OOM的可能原因?
    *   数据库的cursor没有及时关闭
    *   构造Adapter没有使用缓存contentview
    *   RegisterReceiver()与unRegisterReceiver()成对出现
    *   未关闭InputStream outputStream
    *   Bitmap 使用后未调用recycle()
    *   static等关键字
    *   非静态内部类持有外部类的引用　context泄露


java中原子类
阻塞队列
js中闭包
python闭包


innodb

redis，数据类型，消息队列，过期时间问题

职责链模式

缓冲区溢出及其影响

内存中页的大小，为什么是这么大


问了一下流量控制，还是很隐晦，当时大概问的是“一个服务器有很多TCP连接，然后某一时刻他可能来不及处理接受到的数据，这时候该怎么办？”。坦白说刚开始听到我是比较懵B的，但是仔细想过之后发现这好像就是流量控制，所以很流利的回答了流量控制，顺道说了一下原理。


3. 开始了算法，先问我二叉树学过吗，然后让我设计一个节点，再然后让我比较两棵树是否相同（手写代码）。现在我才明白，大概是在考我用递归怎么遍历树，我当时写的居然是以按层遍历的方式去遍历树，然后两棵树逐个节点作对比。

死锁，死锁预防，死锁避免，死锁检测

三次握手四次挥手的状态字，为什么3次，为什么4次

3、Redis用在项目中的哪些地方



9、为什么要加volatile关键字，Synchronized锁住了什么，如果在构造函数中使用远程调用是否会发生中断
10、一个二维数组，每行每列都是升序排列，求这个数组中第K小的数
11、5亿条淘宝订单，每条订单包含不同的商品号，每个商品号对应不同的购买数量，求出销量最高的100个商品

、最后撸了个代码，非常简单，判断链表是否有环。。。

4. 手撕代码：按层次遍历二叉树
5. 手撕代码：按层次遍历二叉树（不完全二叉树）节点为null的需要输出null

2. Elastic Search 索引建立
3. Elastic Search 查询的过程
4. Elastic Search集群管理、Master选举，节点特性
