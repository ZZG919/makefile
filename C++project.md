## C++从代码到可执行二进制文件的过程

​    C++和C语言类似，一个C++程序从源码到执行文件，有四个过程，**预编译、编译、汇编、链接**。

#### 预编译

这个过程主要的处理操作如下：

（1） 将所有的#define删除，并且展开所有的宏定义

（2） 处理所有的条件预编译指令，如#if、#ifdef

（3） 处理#include预编译指令，将被包含的文件插入到该预编译指令的位置。

（4） 过滤所有的注释

（5） 添加行号和文件名标识。

#### 编译

这个过程主要的处理操作如下：

（1） 词法分析：将源代码的字符序列分割成一系列的记号。

（2） 语法分析：对记号进行语法分析，产生语法树。

（3） 语义分析：判断表达式是否有意义。

（4） 代码优化：

（5） 目标代码生成：生成汇编代码。

（6） 目标代码优化：

#### 汇编

这个过程主要是将汇编代码转变成机器可以执行的指令。

#### 链接

将不同的源文件产生的目标文件进行链接，从而形成一个可以执行的程序。

链接分为静态链接和动态链接。

静态链接，是在链接的时候就已经把要调用的函数或者过程链接到了生成的可执行文件中，就算你在去把静态库删除也不会影响可执行程序的执行；生成的静态链接库，Windows下以.lib为后缀，Linux下以.a为后缀。

而动态链接，是在链接的时候没有把调用的函数代码链接进去，而是在执行的过程中，再去找要链接的函数，生成的可执行文件中没有函数代码，只包含函数的重定位信息，所以当你删除动态库时，可执行程序就不能运行。生成的动态链接库，Windows下以.dll为后缀，Linux下以.so为后缀

# 后言

这些执行过程可用C++编译器对源文件进行操作，即指令操作。(详见g++.md文件说明)

集合了指令操作即为makefile操作，由于makefile操作不好写，可用cmake生成makefile文件（详见makefile.md,Cmake.md文件）

由于指令操作的集合顺序的先后，可用ninja进行文件的加速（详见ninja.md文件）