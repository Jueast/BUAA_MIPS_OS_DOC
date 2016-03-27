# 内核、启动、printf
  本次OS实验Lab1核心是理解内核和启动的概念，并且在此基础上实现一个简单的标准库函数`printf`。
  而笔者在完成这一系列实验的同时也尝试将这个lab移植到树莓派上，因此另外也会附加一份ARM上的移植教程（在另一个[repo])。
  那么让我们开始吧~
## 内核
  操作系统从最本质来说，也是软件的一种，而操作系统的内核（kernel），担负着软件最核心的功能：内存管理，进程调度，文件系统服务等等。
  因此可以很容易理解，内核在机器上跑起来时也必须是二进制代码（可执行文件，elf）。而由活生生的人去编写二进制代码显然太过折磨，
  这个时候我们就要求助于编译系统来帮助我们理解和实现内核的相关内容。

### 编译环境配置

  这一块内容之所以需要在我们的项目中重点拎出来，是因为我们编写完内核需要的是交叉编译，在x86/x64平台上编译出mips的机械码，因此编译开关，编译器路径的配置就变得比较重要。而我们所有的实验都在Linux环境下完成，因此我们还需要去熟悉Linux相关指令的操作。（偷偷说，Tab大法好！：） ）
> #### Thinking 1.1
  * `ls -l`    
  这条指令主要是显示当前目录下所有文件的详细信息, 出现形式有点像是列表，比较有用的信息是创建时间和文件的权限。
  * `mv test1.c test2.c`   
  一个利用mv的重命名。
  * `cp test1.c test2.c`   
  拷贝覆盖重命名。
  * `cd ..`   
  ..在linux中代表上级目录，另外如~代表用户根目录，-代表上次目录。

--------
> #### Thinking 1.2
  思考grep 指令的用法，例如如何查找项目中所有的宏？如何查找指定的函数名？
  * `grep "#define" 14061102-lab/* -nR`      
  使用-R选项对文件夹中所有文件进行搜索，-n显示行号
  * `grep "function" * -nR`   
  同理

----

> #### Thinking 1.3
思考gcc 的-Werror 和-Wall 两个参数在项目开发中的意义。
* `-Werror` 该选项将所有Warning作为Error报出，提高了安全性。
* `-Wa` 提供所有警告信息，方便对编译的调试。

----

而到编译环境配置的环节，编写Makefile与配置Linker脚本成为了重中之重。其实这两者不过是简单的脚本语言，如果的Linux的相关命令比较熟悉应该并不困难。

>**改进**：其实还有一个更佳的方案是使用CMake来生成Makefile，相比于直接手工编写Makefile，这样的方式更加清晰简明，笔者在树莓派实验早期调试中使用Cmake，之后为了与MIPS lab保持类似，手工编写了Makefile。

对于`include.mk`的修改，只是简单的修改路径，值得一提的为了方便之后的调试，我们不妨将编译工具链目录和gxemul目录通过在`\.bashrc`末尾添加一行

      PATH = $PATH:path/to/compiler:path/to/gxegxemul

来使得我们在调用它们的时候少输几次路径。

### ELF、编译和链接
  这一块内容我觉得指导书讲的非常细致深入，所提供的实验样例也很好，`readelf`、`objdump`等工具可以很好的帮助我们对所汇编编译出的代码进行分析。

> **改进**   
      *  .data 保存已初始化的全局变量和静态变量。  
    *  .bss 保存未初始化的全局变量和静态变量。
>    表述上似有些问题，.bss全名为 Basic service segment/section，
     其实在C环境建立时其中的变量也都会初始化为0，也就是我们常说的全局变量默认为0，原文中宜添加**显式**二字在初始化前。

### 关于printf
  其实这个函数并没有太多可以说的，就是通过传入一个函数指针让`printf`通过这个函数可以进行打印，所要填写的也只是简单的解析格式化字符串。

### 关于Git
  Lab 1 的后半部分主要关注点在Git的使用上，其实作为一个版本控制系统，git本身是非常易用的，第一次接触git的时候，其实按照pull，push等英文原意去理解，也非常的直观，关于git，我觉得比较好的材料是github官网上的指导。他们甚至把常用命令做成了一张卡片来方便随查随用。


  --------
  > #### Thinking 2.1
    14061102@ubuntu:~/gittest$ rm print.c
    14061102@ubuntu:~/gittest$ ls
    14061102@ubuntu:~/gittest$ git rm print.c
    rm 'print.c'
    14061102@ubuntu:~/gittest$ ls
    14061102@ubuntu:~/gittest$ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       deleted:    print.c
    #
  > 这种情况下恢复很简单  

  >     14061102@ubuntu:~/gittest$ git reset --hard HEAD

  > 即可回滚到上一次commit处  
  > 如果需要保留这次的其他修改可以先  
        `git reset HEAD print.c` 取消对`print.c`的跟踪，然后进行一次commit后再进行相关操作即可。

  ----------
  #### Thinking 2.2
  方法同样简单，   `git reset HEAD Tucao.txt`，即可取消track。  
  更好的办法是再添加该文件名进.gitignore，一了百了。 




  ----
