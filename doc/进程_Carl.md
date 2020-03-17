[TOC]

## 前言

格式说明：

==这里我还不理解==   

<font color=#777>*注释*</font>



## 第一章 操作系统理论知识

### 1.1 概述

#### 基本概念
Operating System：A program that acts as an intermediary between a user of a computer and the computer hardware.
Resource: Component of limited availability necessary for effective operation



计算机系统的组成部分：

* 硬件
    * 中央处理器（central processing unit, CPU）
        * 运算器
        * 控制器
        * 寄存器（Register）: CPU内部一小块存储区域，用来暂时存放参与运算的数据和运算结果。
    * 内存（memory）
        * 随机存储器（Random Accesss Memory，RAM）
        * 只读存储器（Read Only Memory，ROM）
        * 高速缓存（CACHE）
    * 输入输出设备（I/O devices）
* 操作系统
* 系统程序+应用程序

* 用户
  
* 多道程序系统：内存中同时存放多道程序，交替使用CPU
* 分时系统：多个用户分享使用同一台计算机，多个程序分时共享硬件和软件资源。

操作模式：

* 用户模式
* 监督/管理/系统/特权模式
模式位（mode bit）：表示当前模式的方式。监督模式（0），用户模式（1）

System call: Method used by a process to request an action by the OS
* Process control
* File management
* Device management
* Information maintenance
* Communications

Computer System Type:
* Traditional
* Interactive vs batch
* Real time
* Mobile
* Multiprocessor
* Clustered
* Distributed
* Network Operating System

#### 习题

##### 内存
Q: 请描述一种加强内存保护以防止程序修改其他程序的内存的相关机制
A: 虚拟内存：
* 不允许用户进程修改它的只读代码段
* 不允许用户进程读或修改任何内核中的代码和数据结构
* 不允许用户进程读写其他进程的私有内存
* 不允许用户进程修改任何其他进程共享的虚拟页表
当违反上述条件时，将会将这种异常报告为一个段错误（segmentation fault）

##### 网络
Q: 网络计算机与传统计算机有什么区别？
A: 传统计算机的数据，文件，软件都存储在本机的硬盘中。网络计算机的数据，文件，软件很多都存储在远程服务器上，减轻了软件的维护工作，可在线更新，部分数据的安全型增加，但数据的隐私性令人担忧。
Q: 哪些情况下使用网络计算机更多有利？
A: 需要快速迭代的软件，需要多端访问的数据，需要协同工作开发/写作的文件。（大部分合作的场景网络计算机都具有较大的优势）

##### 安全
Q: 有些计算机系统不支持硬件操作特权模式。能否为这些计算机系统构建一种安全的操作系统？请给出能或不能的理由。
A: 能，采用共识投票机制。去掉删除修改的功能，只保留增加和查询文件的功能。这样任何的操作都是可溯源的。比如一个文件多个用户提交了不同版本，每个用户只需要用他们想用的版本就行了，如同命名空间般划分开工作区域，防止工作间的污染。

Q: 在一个分时系统中，能够像在特殊用途系统中一样确认同样的安全程序？并解释它。
A: 不行，人类设计的任何保护机制都会不可避免的被其他人所破译，因而通过单系统本身无法达到足够的数据，文件安全。

##### 操作系统
Q: 列出下列类型操作系统的基本特点

* 批处理：把相似需求的作业成批的及合起来，通过自动切换作业，大大提高了计算机的利用率
* 交互式：允许用户与系统之间的直接通讯
* 分时：多道程序设计的自然延伸。程序的切换速度非常快，用户能流畅与之交互
* 实时: 对处理器操作或数据流动有严格时间要求
* 网络: 提供许多功能的系统
* 并行：多个紧密通信对策处理器，共享计算机总线，时钟，有时还有内存和外设
* 分布：通过网络提供工鞥呢，共享计算任务并向用户提供丰富的功能
* 集群：将多个CPU集合起来完成任务，由两个或多个独立点的系统耦合起来
* 手持：内存少，处理器速度慢

**思考题**
1.什么是操作系统？请用一句话描述你对操作系统的理解。
2.你对操作系统和用户程序之间的关系有何看法？阐述你的想法。
3.简要列出操作系统覆盖的范畴及每个范畴的核心内容。
4.你为什么要学习操作系统？与本书列出的理由相同吗？简要阐述你的动机。
5.操作系统要对不同的部件进行管理，请论述这些管理之间的异同点。
6.设备管理要达到的目的是什么？
7.有人说设备管理软件（设备驱动程序）因为经常由第三方提供，因此不应该作为操作系统的一部分。你对此有何看法？你认为应该如何判断一个软件是否属于操作系统？
8.请列出程序执行过程中操作系统的介入情况。
9.说操作系统是人造学科，根据是什么？
10.人造学科的特点是什么？它对我们学习操作系统有何帮助？
11.OS需要编译器来编译，而编译器的运行需要OS来支持，那到底是谁先出现谁后出现呢？

### 1.2 进程管理

*软件一方面为了获取资源而竞争，一方面又需要相互合作/交换信息，所以需要并发与保护*

#### 1.2.1 进程

进程与OS的关系==人与政府的关系

**进程是什么？**

* 进程: Active program，系统进程资源分配和调度的独立单位，包含：(example: firefox)
  * one or more **Thread**
    * 等待交互事件
    * 重绘页面
    * 加载网页
  * an **address space**
    * firefox.exe
    * 共享库：页面解析，安全性
    * 栈：线程的局部变量
    * 堆：动态分配的内存
  * zero or more open **file handles** (进程可能调用了多个文件，比如C++进程open file with mode R)
    * 配置文件
    * 字体

![img](https://static.dingtalk.com/media/lADPGojJ6nABDxXMtc0Cjg_654_181.jpg_720x720q90g.jpg?bizType=im)

*进程空间=地址空间：进程要用的所有资源（==这句话是不是有问题？CPU不是资源吗==）*

**进程如何被OS创建？**

1. make PCB
2. initilize ==寄存器==
3. initlize ==页表==
4. 将程序代码从磁盘读进内存
5. 将==处理器==状态设置为“用户态”
6. ==跳转到程序的起始地址（设置程序计数器）==



**进程如何被OS维护？**（os有着一个关于进程的数据库，就像医院在我们出生时记录了所有的个人信息，并且时常维护一样，通过这些信息，政府可以对我们个人进行管理）

* Process Control Block 进程控制块(**PCB**)
  * 进程基本信息（身份证）：进程ID，创建用户ID，创建时间等 
  * 进程家族树指针（父母信息）：父子孙进程
  * 进程资源指针（资产信息）：寄存器，栈指针，当前持有句柄等
  * 进程状态信息指针（当前工作/学习状态，健康状态）：==程序寄存器，状态字==，优先级等
  * 时间统计指针（纳税记录）：所占用CPU时间，子进程所占CPU时间
  * 信号支持
  * 其他需要的信息指针...

进程状态

**process state:**

* new
* new: 进程被创建
* ready: 进程已分配到除CPU以外的所有资源
* running：程序正在执行
* waiting：正在执行的进程由于发生某时（如I/O请求，申请缓冲区失败等）暂时无法继续执行的状态
  * 进程由于阻塞而被OS挂起
  * 进程运行了太久而被操作系统挂起
* terminated

**进程切换：**

Context Switch(上下文切换)：一种将CPU资源从一个进程分配给另一个进程的机制。在切换过程中，操作系统需要先存储当前进程的状态（包括内存空间的指针，当前执行完的指令等等），再读入下一个进程的状态，然后执行此级才能拿



**进程如何死亡？**

* 寿终：进程运行完成而退出
* 自杀：系统因错误而自行退出
* 他杀：系统被其他进程终止
* 处决：系统赢一场而被OS强行终结





**为什么要有进程？**

提高系统利用率

![img](https://static.dingtalk.com/media/lALPGpqNY6c5dHzNAXrNAk0_589_378.png_720x720q90g.jpg?bizType=im)

假设一个进程20%的时间进行CPU计算，80%的时间进行I/O，则CPU的利用率为20%

如有2道编程，利用率=1-80%*80%=36%（CPU只有两个进程同时I/O才处于闲置状态）

3道编程：利用率=1-0.8 * 0.8 * 0.8=48.8%

同理....

12道编程，利用率达到94%，再然后的提升很小，进程切换造成的系统消耗开始明显。





**进程相关的 Command**

```bash
$top	//show process info
$ps aus | grep bash		//找出包含bash名字的进程
$pgrep bash				//同上，但是信息更短
$ps						//现在正在运行的进程
$ps -lF 5257	//show process info
$pmap 5257		//show memory mappings?
$lsof -p 5257	//show open files

```

![threads bash](https://www.ops-class.org/img/slides/figures/threads-bash.svg)

- `UID`: user the process is running as.
- `PID`: process ID.
- `PPID`: parent process ID.
- `PRI`: scheduling priority.
- `SZ`: size of the core image of the process (kB).
- `WCHAN`: if the process is not running, description of what it is waiting on.
- `RSS`: total amount of resident memory is use by the process (kB).
- `TIME`: measure of the amount of time that the process has spent running.

![pmap](https://www.ops-class.org/img/slides/figures/pmap.svg)

![lsof](https://www.ops-class.org/img/slides/figures/lsof.svg)



思考题
1.发明进程的根本动机是什么？它与程序是什么关系？请予以论述。
2.进程带给我们的最大好处是什么？它有什么缺点吗？
3.进程空间是什么意思？它包括哪些东西？它与进程是什么关系？
4.进程有哪3种状态，分别代表什么意思？
5.在进程的6种状态转换里有两种是不存在的，但它们不存在的理由却不一样。请予以解释。
6.在商用操作系统里，进程的状态通常多于3个，请问，设置多于3个进程状态的可能原因是什么？
7.操作系统管理进程的根本手段是什么？
8.进程管理时的两个重要考虑是公平和效率。除此之外，还有什么因素需要考虑吗？
9.进程的产生与消亡与人的出生与消亡有着某种类比性，你能否予以阐述？
10.多道编程是否总能提高CPU的利用率？为什么？
11.有同学认为在进程状态的6种转换中，从就绪到阻塞在理论上可行（操作系统将就绪进程的状态改变为阻塞状态），而从阻塞到运行则在理论上不可行，你同意吗？为什么？
12.分析：在内核态下的进程通常共享一个地址空间，这是为什么？



#### 1.2.2 线程

进程一段时间只能做一件事情，容易被I/O阻塞，即使部分工作不依赖与输入数据，也无法进行，为了解决上述存在的问题，人们发明了线程 



**什么是线程？**

thread: CPU调度和分派的基本单元

分类：

* user level thread: 非常高效，不需要进入内核空间，但并发效率不高
* kernel level thread: 内核可将不同线程更好地分配到不同的CPU，以实现真正的并行计算

线程通信的方式：

* 锁机制
  * 互斥锁
  * 条件变量
  * 读写锁
* 信号量机制
* 信号机制

Q: 进程与线程的**区别**
A:

* 进程有自己的独立地址空间， 线程没有
* 进程是资源分配的最小单位，线程是CPU调度的最小
* 进程和线程通信方式不同。线程通信较为方便，同一进程下的线程共享数据
* 进程上下文切换开销大，线程开销小
* 一个进程挂掉了不会影响其他级才能拿，而线程挂掉了会影响其他线程
* 对进程操作开销大，对线程操作开销小



Q: 进程与线程的**联系**
A:

* 一个线程只能属于一个进程，而一个进程可以有多个线程，但至少有一个线程
* 资源分配给进程，统一进程的所有线程共享该进程的所有资源，但是每个线程拥有自己的栈段，用来存放局部变量和临时变量
* CPU上真正运行的是线程
* 线程执行中需要协作同步，不同级才能拿的线程间要利用消息通信的办法实现同步。



Q: 为什么进程上下文切换比线程上下文切换代价高？

A:因为进程切换分两步，而线程只需做第二步。

1. 切换页目录以使用新得地址空间
2. 切换内核栈和硬件上下文



#### 1.2.3 CPU调度



**CPU调度的目标是什么？**

1. 极小化平均响应时间[^1]
2. 极大化系统吞吐率[^ 2]
3. 保证系统的效率与公平

**短期调度**

一群病人讨论问诊顺序

A: 我先来的，应该是我（先到先服务）

B: 我就问句话，我先（最短作业优先调度）

C: 我是党员（优先级调度）

D: 每个人进去问3分钟，没问完回来继续排队（轮转调度）



* FCFS：First-Come First-Served, 先到先服务调度，公平的算法，根据客户到店的次序，自觉顺序排队，先发出请求的客户被先处理，类似于食堂打饭的排队方式（常用于后台队列）

* SJF: Shortest-Job-First, 最短作业优先调度，效率的算法，能最快能减少等待队伍的长度，让进程依据被处理所需要耗费的时间，最快的先被处理，类似于考试时的一种排序方式，先做简单的题目。
* Priority-scheduling, 优先级调度，效率的算法，能保证系统的健壮性，优先级的定义分为内部与外部，内部由程序所耗费的资源决定，外部由进程重要性，用户等级等决定。类似于医院资源的分配，优先分配给情况紧急的人，或者重要的人。
* RR: Round-Robin, 轮转调度, 公平的算法，FIFO队列中的程序轮流地等时地使用CPU资源，类似于发扑克牌。（常用于前台进程，每隔一个时间片刷新页面）

* Multilevel queue, 多级队列调度，将所有进程分成不同的大类，每个大类为一个优先级

  * 如果两个进程处于不同的大类，高优先级的进程先执行
  * 如果两个进程处于同一个大类，采用RR轮转执行

* 多级反馈队列调度

* 其他调度算法

  * 保证调度 Guaranteed Scheduling
  * 彩票调度 Lottery Scheduling
  * 用户公平调度 Fair Share Scheduling Per User
  * 实时调度算法
  * EDF调度算法

  ![多级队列调度](http://c.biancheng.net/uploads/allimg/181106/2-1Q106161105337.gif)

![img](https://static.dingtalk.com/media/lADPGpNyZmRBBwLM380Bog_418_223.jpg_720x720q90g.jpg?bizType=im)

<center>多级队列调度</center>



**中期调度** ==举例==



**长期调度** ==举例==



Q: 短期，中期，长期调度之间的区别是什么

A:

* 短期调度（CPU调度）
  * 决定哪个进程接下来占用CPU，调用十分频繁
* 中期调度
  * used especially with time-sharing systems as an intermediate scheduling level. 
  * A swapping scheme is implemented to remove partially run programs from memory and reinstate them later to continue where they left off.
* 长期调度（工作调度）
  * 决定哪个进程进入准备队列



**思考题**
1.进程调度追求的目标是什么？
2.程序使用CPU的模式有哪几种？分别有何特点？
3.短任务优先和优先级调度是否是同一种调度，为什么？
4.如果想让某个进程获得50%的运行机会，请问应该使用哪一种调度策略？
5.假定有A、B、C 3个进程，A、B均是纯计算进程，分别需要使用CPU计算50毫秒和100毫秒，而C每计算1毫秒后进行9毫秒的输入输出操作，并这样重复10次。
a）按顺序C、A、B几乎同时到达系统，给出FCFS的调度情况并计算系统响应时间。
b）按顺序B、A、C几乎同时到达系统，给出FCFS的调度情况并计算系统响应时间。如果使用时间片轮转，情况又如何？这里假设时间片大小为10毫秒。
6.仔细讨论保障调度和时间片轮转调度的异同点。它们的根本区别是什么？
7.优先级倒挂是怎么回事？它有什么危害？
8.请分析讨论解决优先级倒挂的3种办法，哪一种的优势最明显。
9.请从人类的中庸之道分析讨论现代商用操作系统所采取的混合调度策略。
10.试阐述适度性在进程调度中的作用，并举例予以说明。
11.调研某一商业操作系统的进程调度策略，它与本章阐述的调度策略有何异同？
12.如果一个进程在最后一个CPU任务结束后有一个很长的I/O操作，则在计算CPU响应时间的时候是否需要计入该I/O操作的耗时？请说明你的理由。
13.假如时间片轮转的时间片的大小是10毫秒，进程A在1毫秒的时候阻塞或者结束，进程B接着用该时间片，则进程B拥有的是9毫秒的时间片，还是10毫秒？为什么？
14.如果一个任务因为I/O操作阻塞了，其恢复后需要排到队尾等待吗？



#### 1.2.4 进程同步

主要任务：对多个相关进程在执行次序上进行协调，以使并发执行的诸进程之间能有效地共享资源和相互合作，从而使进程的执行具有可再现性。

同步机制遵循的原则 ：

1. 空闲让进
2. 忙则等待
3. 有限等待
4. 让权等待

进程通信 Inter-Process Communication , **IPC**（共7种方式）：进程之间的交互

（人类通信的方式：拥抱，打手势，发电报，写信，说话，画画）

* 对白（信息丰富，流传输）：
  * 管道[^3]：线性字节数组
  * 命名管道[^4]
  * 套接字[^5]
* 电报（迫使对方回应，即时通信，信息量小）
  * 信号[^6]，singal
* 铁轨旁的红绿灯（只有0和1两个状态）
  * 信号量[^7]，semaphore
* （大量数据）
  * 共享内存[^8]，shared memory
* 信件（*生产者与消费者模型*）
  * 消息队列[^9]

* 其他
  * 共享存储器系统 -- 剪贴板
  * 消息传递系统 -- ROS

[^3]:管道：单向，先进先出，无结构，固定大小的字节流，把一个进程的标准输出和另一个进程的标准输入连接到一起，只能用于父子进程
[^4]: 命名管道：
[^5]: Socket：一种不同机器间通信的机制
[^6]: 信号：
[^7]: 信号量：存在问题：程序编写困难，执行效率低下
[^8]: 共享内存
[^9]: 消息队列：一个在系统内核中用来保存消息的队列，以消息链表的形式出现，克服了信号传递信息少，管道只能承载无格式字节流以及缓冲区大小受限等缺点。

消息传递存在的问题：

* 消息丢失
* 身份识别
* 效率

网络协议正是为了解决上述问题而出发设计的，比如TCP协议提高了网络传输的可靠性，数字签名和加密技术解决身份识别的问题、







**通信相关的command**

==sort和grep的关系是什么？兄弟进程？==

```bash
$ sort<file1|grep lin	//管道，两个进程 sort,grep  通过 | 创建管道，将 sort 的结果作为grep的输入
$ kill 					//信号，SIGTERM, 软杀死，会释放进程占用的资源，容易被阻塞(被默认为 kill -15)，无法杀死后台进程，守护进程等
$ kill -9				//信号，SIGKILL，硬杀死，exit信号不会被系统阻塞
```



**通信相关的API**

```c++
int pp[2];			
pipe(pp);			//create pipe，进程之间必须存在某种关系，比如父子进程
if(fork()==0){		//create subprocess
    read(pp[0]);	//read from parent process
    ...
}else{
    write(pp[1]);	//write to subprocess
    ...
}				
```



线程同步的方式：

* 临界区[^11]: 可能造成竞争的共享代码段或资源
* ==互斥量==
* 信号量
* 事件
* 管程，monitor, =锁+条件变量，有wait和signal两个操作



**思考题**
1.进程为什么需要通信？
2.进程通信要达到的目的是什么？
3.消息队列和管道有何异同？
4.管道的物理实体是什么？
5.套接字和管道的异同点是什么？可以用管道实现套接字的功能吗？
6.请调查某个商用操作系统的套接字实现，并予以讨论。
7.在本章介绍的所有通信机制里面，哪一种机制支持的信息交换量最大？
8.试将本章介绍的通信机制按照出现的逻辑顺序排列出来，并简要说明其逻辑关系。
9.有同学认为本章介绍的信号和信号量实际上是一回事情，你同意吗？为什么？
10.请举出一个需要进程通信的编程实例。



#### 1.2.5 死锁

锁：线程保护资源的方法

（有两个老师都想用钢琴教室上课，A老师进门后直接将门锁了，B老师就进不来了，无法与其抢夺资源  [ 锁的互斥性 ] ）

死锁：

* 概念：有一组线程，每个线程都在等待一个事件的发生，而这个事件只能由改组县城里面的另一线程发出，则称这组线程发生了死锁  *[这里的事件通常指资源的释放]*
* 原因
* 必要条件--->消除死锁的方法
  * 互斥|资源有限-->把资源无限增加或者可共享
  * 占有并等待-->一个线程必须一次性请求所有资源
  * 非抢占-->允许抢占资源
  * 循环等待-->锁
* 解除与预防
  * 视而不见
  * 检测与修复
  * 动态避免
  * 静态避免

![img](https://static.dingtalk.com/media/lADPGoGu7THBaqfNA3HNBQA_1280_881.jpg_720x720q90g.jpg?bizType=im)

<center>交通瘫痪--现实生活中的死锁

**哲学家就餐问题**：

![img](https://static.dingtalk.com/media/lADPGoU8a9b6Zi7NATbNAUw_332_310.jpg_720x720q90g.jpg?bizType=im)

<center>哲学家就餐问题

哲学家吃饭需要左右两边各一个筷子

```
哲學家算法：
	1.等待左边的筷子可用，然后拿起左边的筷子
	2.等待右边的筷子可用，然后拿起右边的筷子
	3.吃饭
	4.放下两个筷子
```



**银行家算法**：





[^21]:生产者消费者模型

A进程 ~~ 生产者 ~~ 送货员

B进程 ~~ 消费者 ~~ 学生

内存缓冲区 ~~ 商店 ~~ 售货机

![img](https://static.dingtalk.com/media/lADPGpqNY7Ifd1svzQGw_432_47.jpg_720x720q90g.jpg?bizType=im)

```
if 学生 发现 售货机 空了
	A方案：在售货机前 等待，直到 送货员 来装货
	B方案：回宿舍睡觉，等售货员装货了再去买(需要wakeup,知道售货机的状态，信号量)
if 售货员 发现 售货机 满了
	A方案：等人买走一些，再将售货机填满
	B方案：回家睡觉，等有人买了再来补货（需要wakeup）
```



### 1.3 内存管理

#### 1.3.1 内存管理策略
Buffer: Area of memory that stores data
Cache: Area of fast memory that stores copies of data

* 指令寄存器（instruction register）
* 内存（memory）/ 随机访问内存（random access memory, RAM）：可被CPU和I/O设备共同快速访问的数据仓库
* 辅存/二级存储器（secondary storage）
    * 磁盘（magnetic disk）：读取速度慢，价格低
    * 硬盘
* 三级存储器（tertiary storage）
    * 磁带驱动器+磁带
    * CD/DVD驱动器+光盘

具体活动

* 记录内存中的哪部分正在被使用及被谁使用
* 当有内存空间是，决定哪些进程可以装入内存
* 根据需要分配和释放内存空间



#### 1.3.2 虚拟内存管理



### 1.4 存储管理



#### 1.4.1 文件系统

基本概念

文件：程序+数据

具体活动

文件管理：

* 创建和删除文件
* 创建和删除目录来组织文件
* 提供操作文件和目录的原语
* 将文件映射到二级存储上
* 在稳定存储介质上备份文件

硬盘管理：

* 空闲空间管理
* 存储空间分配
* 硬盘调度



#### 1.4.2 文件系统实现

文件：逻辑外存的最小分配单元，数据只有通过文件的形式才能存到外存

什么是逻辑外存？

外存-->外部存储器-->寄存器才算内部存储器？

文件操作：

* read
* write
* execute
* add
* delete
* list

访问控制列表 Access-Control List, ACL: 





#### 1.4.3 大容量存储结构



#### 1.4.4 I/O系统

现况：

* 软件和硬件接口日益标准化
* I/O设备种类的不断扩展

总线（bus）=线路 + 协议（关于传输信息）





### 1.5 保护与安全
#### 基本概念
* 保护：控制进程或用户对计算机资源的访问的机制
* 安全（security）：防止系统不受外部或内部攻击
    * 病毒/蠕虫
    * 拒绝服务攻击：使用所有的系统资源以致合法的用户不能使用
    * 身份偷窃
    * 服务偷窃：未授权的系统使用
* 升级特权（escalate privilege）
Deadlock: A situation in which two or more competing actions are each waiting for the other to finish and thus neither ever does.


#### 具体活动

### 1.6 案例研究

OS设计准则：

* Separate policy from mechanism
* Facilitate control or sharing by adding a level of indirection

### 1.7 附录

#### 名词解释

沙箱：一个安全的虚拟操作系统，方便黑客进行各种可疑软件的测试。

随机存取（对标直接存取 Sequential Access）：当存储器中的消息被读取或写入时，所需时间与信息所在位置无关

易失性：电源关闭后，RAM上的数据会消失

随机存取存储器  RAM, Random Access Memory：

只读存储器  ROM，Read-Only Memory：

全双工 Full Duplex: 可同时进行信号的双向传输，like 微信视频

单工 Simplex Communication: 数据传输单向，like 汽车的单行道

半双工：数据可以双向传输，但不同同时，like 对讲机

Kernel: OS中一直运行着的进程

适度性：响应时间和期望值相匹配，但不要超越用户的期望，因为一方面会增加系统设计的难度，另一方面不会提高用户的满意度

页目录：

硬件上下文：

地址空间：

竞争，race：两个或多个线程争相执行同一段代码或访问同一资源的现象

[^1]: 平均响应时间：用户发出命令和看到某种结果之间所花费的时间
[^ 2]: 系统吞吐率：单位时间内完成尽可能多的程序

#### 其他

理解操作系统概念的一种方法：

* hiding undesirable properties
* adding new capailities
* organizing Information

思考形式：

* What undesirable properties do      hide?  （底层，硬件细节）
* What new capabilities do      add?  （物理上并不存在，有助于组织自身）
* What information do      help organize?  （帮助用户）

Example:File

* What undesirable properties do file system hide?
  * Disks are slow
  * Chunks of storage are actually distributed all over the disk
  * Disk storage may fail!
* ==What new capabilities do files add?==
  * Growth and shrinking  
  * Organization into directories (添加了一个物理上并不存在的目录结构)
* What information do files help organize?
  * 所有权和读写许可
  * 进入时间，修改时间，类型等

抽象关系：

* Thread -- CPU
* Address spaces -- memory
* Files -- Disk



### 1.8 学习想法

**必然性**

要是早点学操作系统就好了  以前写币，搭服务器，写slam都会遇上很多linux的问题  花很多时间搜些网上的解决方案一个个暴力试错，模模糊糊解决问题，不知道浪费了多少时间，操作系统真的是和英语一样  越早学习  越是受益无穷

**I/O系统**

整个互联网像是一个巨大的操作系统，我们手中的手机从一些角度来看变成了一个服务请求端。打印机，咖啡机，自行车，各式各样的服务都变成了系统输出的终端。互联网操作系统输入了人的需求，输出了服务，被资本的欲望驱使着，目标明确地滚动着经济雪球。





## 第二章 Linux理论知识



### Linux图解

这张图讲解的是Linux内部的结构，那么我们来看一看这张图的结构。

![img](https:////upload-images.jianshu.io/upload_images/1177386-0cda0bad9bf5e888.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

全图纵观

首先，我们来看一下全图。
 这个图分为三层，最下层是文件柜和大量的文件，意为`文件系统`，我们可以看到，Linux 底层的文件系统都是分立的，一个个的文件柜，里面放着不同的文件，就如同我们所了解到的。Linux 的文件系统是基于目录的文件系统。

![进程和操作系统之间的通信](https://ooo.0o0.ooo/2017/03/09/58c10e7cbc378.png)

进程和操作系统之间的通信

在第二层，我们可以找到一个台阶下到下面的文件系统，进程们可以通过这个台阶，和文件系统实现通信和操作。

看完了底层和第二层的交流。接下来我们来看一看二层的一些对外的出入口

![img](https:////upload-images.jianshu.io/upload_images/1177386-ae117996732669b5.png?imageMogr2/auto-orient/strip|imageView2/2/w/363/format/webp)

80端口

这里有一只小企鹅，站在门口，门口上面写着`80`，说明他看守的是80端口，也是我们日常使用的 http ( 网站 )的端口。
 在小企鹅的头上顶着一个羽毛，这个是  Http  服务软件 apache 的 logo。

![img](https:////upload-images.jianshu.io/upload_images/1177386-5412354a29eb6d6c.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/369/format/webp)

Apache



在小企鹅的身上的数字，说明他是1341，意指 PID ( Process ID).

> 综合来说，这个图表现了一个 pid 为 1341 的 http 进程在80端口上对外提供服务。

![img](https:////upload-images.jianshu.io/upload_images/1177386-74d8fdb303c22e18.png?imageMogr2/auto-orient/strip|imageView2/2/w/158/format/webp)

ssh 服务

这里也站着一个小企鹅，他的身上写着 52，说明他是第52 个小企鹅，PID 为 52，他旁边的门上写着 22，说明他看守的是 22 端口。

![img](https:////upload-images.jianshu.io/upload_images/1177386-18b354b5a77f593f.png?imageMogr2/auto-orient/strip|imageView2/2/w/157/format/webp)

21端口

除了上述的两个端口意外，这里还有一个无人看管的21端口，这里的无人看管，就意味着不安全，可能会被人所攻击、利用。

![img](https:////upload-images.jianshu.io/upload_images/1177386-0488c5ee57bbfc34.png?imageMogr2/auto-orient/strip|imageView2/2/w/423/format/webp)

进程表

在2层的左下角，有一个 process table（进程表） ，整个 linux 的进程都会在这里等待用户的召唤，当用户召唤时，他们就去执行自己的工作。当用户不召唤时，他们就在这里等待，直到用户召唤。

![img](https:////upload-images.jianshu.io/upload_images/1177386-122d3c9de5f2c6b5.png?imageMogr2/auto-orient/strip|imageView2/2/w/135/format/webp)

watchdog

在 process table 旁，蹲着一个 watchdog （监控），他会随时监控 进程的状态，如果进程的状态不对，就会去咬小企鹅。直到小企鹅的状态变得正常。

![img](https:////upload-images.jianshu.io/upload_images/1177386-6ddfb15438f0448c.png?imageMogr2/auto-orient/strip|imageView2/2/w/122/format/webp)

wine

有一只小企鹅手里拿着一杯红酒，他是 wine（在linux上模拟windows软件的应用）

![img](https:////upload-images.jianshu.io/upload_images/1177386-0db29b87177c68ae.png?imageMogr2/auto-orient/strip|imageView2/2/w/194/format/webp)

cron

cron 小企鹅总是看着自己的手表，时刻关注着事件，以免耽误了事情的发展。在合适的时候，联系其他的小企鹅，来做事。

![img](https:////upload-images.jianshu.io/upload_images/1177386-f17be01c3a6b41bd.png?imageMogr2/auto-orient/strip|imageView2/2/w/311/format/webp)

pipline

通道是小企鹅们之间传递消息的工具，借助通道，一个小企鹅，可以把自己准备好的东西，传递给下一个小企鹅。让下一个小企鹅，继续做事。而不需要跑来跑去通知不同的小企鹅。

![img](https:////upload-images.jianshu.io/upload_images/1177386-2709ca0050efdc44.png?imageMogr2/auto-orient/strip|imageView2/2/w/367/format/webp)

tty

在三层，上面挂着6个 tty（终端），每个终端，都在执行着不同的事情，每个小企鹅，在需要工作时，都要来到这里，执行自己要做的事情



作者：白宦成
链接：https://www.jianshu.com/p/c40dd538067f
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





## 第三章 项目实践

### Plan & 进度追踪

#### 第一阶段 Study

OSC + LKD《操作系统概念》第九版 (Operating System Concept， OSC）是操作系统领域的名著，内充丰富而详实，也提供了大量的习题供我们巩固和提高，实在是不可不读的好书。
 《Linux内核设计与实现》第三版 (Linux Kernel Development) 被誉为Linux内核开发的四大名著之一，书中处处是干货，没有半句废话，不得不读。
**第一阶段目标** 是通过 OSC + LKD 对操作系统有一个宏观的认知，清楚地知道操作系统由哪些模块组成，各种概念的具体定义和运作方式。
 OCS一书分为 概念、进程管理、内存管理、存储管理、保护与安全和案例研究六个部分，因此阶段规划也按照这六个部分进行排期。第一阶段以OCS一书为主线进行，因为LKD书中不少章节可以作为OCS的扩展阅读，所以建议在阅读OCS的时候配合LKD进行即可，不单独为LKD排期。

**概念** 02月18日 - 02月23日

- [x] 目标: 内容 + 习题 + 编程题 + 编程项目

**进程管理** 02月24日 - 03月15日

**STL** 

- [ ] 目标: 内容 + 习题 + 编程题 + 编程项目

好难呀= =

**内存管理** 03月16日 - 03月25日

- [ ] 目标: 内容 + 习题 + 编程题 + 编程项目

**存储管理** 03月26日 - 04月15日

- [ ] 目标: 内容 + 习题 + 编程题 + 编程项目

**保护与安全** 04月16日 - 04月30日

- [ ] 目标: 内容 + 习题 + 编程题 + 编程项目

**案例研究** 05月03日 - 05月10日

- [ ] 目标: 内容 + 习题 + 编程题 + 编程项目

#### 第二阶段 Practice

#### 第三阶段 强化，总结与展望









