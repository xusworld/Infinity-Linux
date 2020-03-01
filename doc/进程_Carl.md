[TOC]

## 第一章 操作系统理论知识

### 概述

#### 基本概念
Operating System：A program that acts as an intermediary between a user of a computer and the computer hardware.
Resource: Component of limited availability necessary for effective operation

计算机系统
* 组成部分：
    * 硬件
        * 中央处理器（central processing unit, CPU）
            * 运算器
            * 控制器
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

### 进程管理
#### 进程

* Process: Active program，系统进程资源分配和调度的独立单位
* Kernel: contains basic  function (one process active all the time)
* Process Control Block(PCB)
	* A Data structure created by the OS for each process, with:
		* process state: 
			* new
			* ready：进程已分配到除CPU以外的所有资源
			* running：程序正在执行
			* waiting：正在执行的进程由于发生某时（如I/O请求，申请缓冲区失败等）暂时无法继续执行的状态
			* terminated
		* process number
		* program counter
		* registers
		* memory limits
		* list of open files
		* ...
* Program: passive entity stored on disk(executable file)
#### 线程

thread: CPU调度和分派的基本单元

分类：

* user level thread: 非常高效，不需要进入内核空间，但并发效率不高
* kernel level thread: 内核可将不同线程更好地分配到不同的CPU，以实现真正的并行计算

Q: 进程与线程的区别
A:

* 进程有自己的独立地址空间， 线程没有
* 进程是资源分配的最小单位，线程是CPU调度的最小
* 进程和线程通信方式不同。线程通信较为方便，同一进程下的线程共享数据
* 进程上下文切换开销大，线程开销小
* 一个进程挂掉了不会影响其他级才能拿，而线程挂掉了会影响其他线程
* 对进程操作开销大，对线程操作开销小

Q: 进程与线程的联系
A:

* 一个线程只能属于一个级才能拿，而一个进程可以有多个线程，但至少有一个线程
* 资源分配给进程，统一进程的所有线程共享该进程的所有资源，但是每个线程拥有自己的栈段，用来存放局部变量和临时变量
* CPU上真正运行的是线程
* 线程执行中需要协作同步，不同级才能拿的线程间要利用消息通信的办法实现同步。

Q: 为什么进程上下文切换比线程上下文切换代价高？

A:因为进程切换分两步，而线程只需做第二部。

1. 切换页目录以使用新得地址空间
2. 切换内核栈和硬件上下文

Q: 页目录是什么？

Q:硬件上下文是什么？

Q:地址空间是什么？



#### CPU调度

Q: what's the Difference between short-term scheduler, medium-term scheduler, long-term scheduler?

A:

* short-term scheduler
  * also called CPU scheduler
  * select which process should be executed next and allocate CPU 
  * invoked very frequently
* medium-term scheduler
  * used especially with time-sharing systems as an intermediate scheduling level. 
  * A swapping scheme is implemented to remove partially run programs from memory and reinstate them later to continue where they left off.
* long-term scheduler
  * also called job scheduler
  * select which processes should be brought into the ready queue
  * invoked infrequently

#### 进程同步

主要任务：对多个相关进程在执行次序上进行协调，以使并发执行的诸进程之间能有效地共享资源和相互合作，从而使进程的执行具有可再现性。

同步机制遵循的原则 ：

1. 空闲让进
2. 忙则等待
3. 有限等待
4. 让权等待

进程通信

* 低级通信
* 高级通信
  * 共享存储器系统 -- 剪贴板
  * 消息传递系统 -- ROS
  * 管道通信系统

信号量：计数器，用来控制多个进程对共享资源的访问，是一种常用的锁机制，防止多进程同时访问共享资源

管道：单向，先进先出，无结构，固定大小的字节流，把一个进程的标准输出和另一个进程的标准输入连接到一起。

消息队列：一个在系统内核中用来保存消息的队列，以消息链表的形式出现，克服了信号传递信息少，管道只能承载无格式字节流以及缓冲区大小受限等缺点。

共享内存：

Socket：一种不同机器间通信的机制

Context Switch(上下文切换)：一种将CPU资源从一个进程分配给另一个进程的机制。在切换过程中，操作系统需要先存储当前进程的状态（包括内存空间的指针，当前执行完的指令等等），再读入下一个进程的状态，然后执行此级才能拿

#### 死锁





### 内存管理
#### 基本概念
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


#### 具体活动

* 记录内存中的哪部分正在被使用及被谁使用
* 当有内存空间是，决定哪些进程可以装入内存
* 根据需要分配和释放内存空间

### 存储管理
#### 基本概念
文件：程序+数据



#### 具体活动
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

### 保护与安全
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

### 案例研究

## 第二章 Linux理论知识



### 





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







## 第四章 STL

