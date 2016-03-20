# 内核、启动、printf
  本次OS实验Lab1核心是理解内核和启动的概念，并且在此基础上实现一个简单的标准库函数`printf`。
  而笔者在完成这一系列实验的同时也尝试将这个lab移植到树莓派上，因此本实验报告中也会涉及一些对比的相关内容。
  那么让我们开始吧~

  ##　内核
  操作系统从最本质来说，也是软件的一种，而操作系统的内核（kernel），担负着软件最核心的功能：内存管理，进程调度，文件系统服务等等。
  因此可以很容易理解，内核在机器上跑起来时也必须是二进制代码（可执行文件，elf）。而由活生生的人去编写二进制代码显然太过折磨，
  这个时候我们就要求助于编译系统来帮助我们理解和实现内核的相关内容。
  
  ### 编译环境配置
  这一块内容
  