### 目标
操作系统是程序员的三大浪漫之一，作为一个程序员，你真的了解操作系统吗？下面是几个问题请你来回答一下

1. 进程是如何实现？如何调度？
2. 进程在内存中如何换入换出？
3. 进程间通信实现方式？如何实现共享内存策略，如何设计消息传递策略？
4. 如何设计一个文件系统？
5. 虚拟内存系统怎么实现？

### 开发要求

由于开发Linux Kernel 0.11不需要对
* [x] Linux操作系统
* [x] Git
* [x] Linux commands
* [x] C
* [x] 汇编

### 任务排期
1991年，芬兰天才编程少年Linus在发布了Linux Kernel0.11，这份总代码量不过两万行的代码后来深刻的改变了操作系统发展的格局。侯捷先生曾经说过“源码面前，了无秘密”，阅读别人的代码也是一次和作者神交的过程，因而才有了阅读Linux Kernel源码，重写Linux Kernel源码甚至创新Linux Kernel源码的“疯狂想法”。即使是Linux Kernel 0.11依然是一块硬骨头，营养丰富但也不见得啃得动。

考虑到参与这个项目的同学都不是全职投入到这个项目中，因此在计划之初就摒弃了“大跃进”式的开发规划，如“30天自制操作系统”、“21天Linux Kernel入门到精通”等。前文中也已经介绍，该项目的目标是让每一个committer能深入理解操作系统内核，理解一行指令执行之后究竟发生了什么。因此，该项目预计持续六个月的时间，从现在到今年八月底，前后共计二百天时间。

表面上看，二百天时间阅读一万多行代码绰绰有余，可操作系统博大精深，肯定一些细节而巧妙的设计难住我们，也会有各种各样的诱惑让我们放弃，毕竟“坚持”这两个字是反人性的，不过我坚信这半年的付出能够换来终生受益。前途路远，道阻且长，且行且坚持。

#### 第一阶段 OSC + LKD
《操作系统概念》第九版 (Operating System Concept， OSC）是操作系统领域的名著，内充丰富而详实，也提供了大量的习题供我们巩固和提高，实在是不可不读的好书。
《Linux内核设计与实现》第三版 (Linux Kernel Development) 被誉为Linux内核开发的四大名著之一，书中处处是干货，没有半句废话，不得不读。
**第一阶段目标** 是通过 OSC + LKD 对操作系统有一个宏观的认知，清楚地知道操作系统由哪些模块组成，各种概念的具体定义和运作方式。
OCS一书分为 概念、进程管理、内存管理、存储管理、保护与安全和案例研究六个部分，因此阶段规划也按照这六个部分进行排期。第一阶段以OCS一书为主线进行，因为LKD书中不少章节可以作为OCS的扩展阅读，所以建议在阅读OCS的时候配合LKD进行即可，不单独为LKD排期。


**概念** 02月18日 - 02月23日

目标: 内容 + 习题 + 编程题 + 编程项目

- [x] 董佳旭
- [x] 林俊旭
- [x] 朱赛赛
- [x] 王素文

**进程管理** 02月24日 - 03月31日

目标: 内容 + 习题 

- [x] 董佳旭
- [x] 林俊旭
- [x] 朱赛赛

**内存管理** 04月01日 - 04月15日

目标: 内容 + 习题 

- [ ] 董佳旭
- [ ] 林俊旭
- [ ] 朱赛赛
 
 **存储管理** 04月16日 - 04月30日
 
目标: 内容 + 习题 

- [ ] 董佳旭
- [ ] 林俊旭
- [ ] 朱赛赛

 **保护与安全** 05月01日 - 05月15日
 
目标: 内容 + 习题 

- [ ] 董佳旭
- [ ] 林俊旭
- [ ] 朱赛赛

**案例研究** 05月15日 - 05月20日
 
目标: 内容 + 习题

- [ ] 董佳旭
- [ ] 林俊旭
- [ ] 朱赛赛

### 开发要求
这个部分还需要大家一起讨论一下，暂时的想法组织周会，分享作业和代码到项目中。

### 进阶设想
* [ ] 开始阅读《UNIX环境高级编程》
* [ ] Linux Kernel 2.4
* [ ] Linux driver development






