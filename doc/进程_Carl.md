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
* Proces control
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
#### 基本概念
* Process: Active program
* Kernel: contains basic  function (one process active all the time)
* Process Control Block(PCB)
	* A Data structure created by the OS for each process, with:
		* process state: 
			* new
			* ready
			* running
			* waiting
			* terminated
		* process number
		* program counter
		* registers
		* memory limits
		* list of open files
		* ...
* Program: passive entity stored on disk(executable file)
#### 习题
1. The Difference between short-term scheduler, medium-term scheduler, long-term scheduler.
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

2. Use C language and fork() to generate Fibonacci sequence

```c++
#include<stdio.h>
#include<unistd.h>
#include<sys/wait.h>
int main(){
	pid_t pid;
	int fib0=0,fib1=1,i=1;
	unsigned int num;
	scanf(“%d”,&num);
	pid=fork();
	if(pid==0){ 	//subprocess
		if(num<=0) printf(“uncorrect num inputed”);
		if(num==1) printf(“%d”,fib0);
		if(num==2) printf(“%d”,fib1);
        while(i<num){
            ... //有点不对，我找找答案
        }
	}else if(pid==-1){
		printf(“generate subprocess unsuccess\n”);
        return 0;
	}else{
		Wait(NULL);
        return 0;
	}
}	
```



### 内存管理
#### 基本概念
Buffer: Area of memory that stores data
Cache: Area of fast memory that.stores copies of data
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

