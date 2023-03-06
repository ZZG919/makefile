**--version**

显示g++ 的版本 。

**--target-help**

显示特定平台环境的参数支持。比如嵌入式领域使用的avr-g++会对MCU 支持一些优化参数。

**-std=<语言标准>**

如：-std=c++11 ，使用C++11标准

**-ansi**

使用ANSI 标准，禁止GNU 标准特性，如 asm inline typeof 关键字,以及UNIX,vax等预处理宏。

**-funsigned-char**      #将程序中的char解析为unsigned char
**-fno-signed-char**     #将程序中的char解析为非signed char
**-fsigned-char**        #将程序中的char解析为signed char
**-fno-unsigned-char**  #将程序中的char解析为非unsigned char

你的编译器中，char类型是 signed char 还是 unsigned char?不太清楚。嗯，所以如果对范围敏感，应该明确定义unsigned char ，或者是 signed char 。上面4个参数就是指定g++将char解释为什么类型。

**-O0** 
**-O1** 
**-O2** 
**-O3**
取英文单词Optimize（意为：优化）的第一个字母O,不是零哦。
编译器的优化选项的4个级别，-O0表示没有优化,-O1为缺省值，-O3优化级别最高。
优化的是什么呢？生成的程序的大小和程序的执行速度。

**-w**

编译时，不显示任何警告消息。

**-Wall**

编译时，显示所有出现的警告消息。

C/C++源代码的编译过程：

![img](https://raw.fastgit.org/ZZG919/Typora_Picture/master/img/202212291743236.png)

**-o <file>**

输出编译后的结果到指定的文件file中。windows下默认编译输出a.exe，而linux则默认是a.out。-o不仅可以指定输出的可执行文件，还可以指定中间文件的输出，后面会用。

g++ hello.cpp -o hello.exe      # 输出指定的可执行文件 hello.exe

g++ -o hello.exe hello.cpp     # 同上

g++ hello.cpp            #windows下输出的是a.exe，linux下输出的是a.out

 **-E**

对源文件进行预处理，预处理后生成.i（ 或者是 .ii）文件。通过此命令可以查看于处理器是如何“修改”我们的.cpp源文件的，以理解预处理的工作机制。生成的是文本文件。 

g++ -E -o hello.i hello.cpp      #对hello.cpp进行预处理生成hello.i文件

**-S**

只进行预处理和编译，编译是C++编译器的核心操作，其结果就是将C++代码中转译为汇编代码，生成.s汇编文件。生成的依然是文本文件。

g++ -S -o hello.s hello.cpp     #生成汇编文件hello.s

**-c**

只进行 预处理， 编译，汇编操作，生成.o (.obj)文件，不进行链接。生成的是二进制文件。

g++ -c -o hello.o hello.cpp

**-save-temps**

顾名思义，就是保留编译产生的中间文件，使用这个参数，就没必要将前面的参数 -E -S -c 一个一个地使用了。

**-M , -MD**

输出源文件的#include指令 （ #include<head_file> , #include'head_file' ） 引起的所有包含文件依赖。

使用-M参数的效果：

-M参数可以查看源文件实质上包含的所有头文件

```
» g++ -M cmd.cpp    

cmd.o: cmd.cpp e:\c++\mingw\lib\gcc\mingw32\4.8.1\include\c++\cmath \

 e:\c++\mingw\lib\gcc\mingw32\4.8.1\include\c++\mingw32\bits\c++config.h \

 e:\c++\mingw\lib\gcc\mingw32\4.8.1\include\c++\mingw32\bits\os_defines.h \

 e:\c++\mingw\lib\gcc\mingw32\4.8.1\include\c++\mingw32\bits\cpu_defines.h \

 e:\c++\mingw\lib\gcc\mingw32\4.8.1\include\c++\bits\cpp_type_traits.h \

 e:\c++\mingw\lib\gcc\mingw32\4.8.1\include\c++\ext\type_traits.h \

 e:\c++\mingw\include\math.h e:\c++\mingw\include\_mingw.h \

 e:\c++\mingw\include\sdkddkver.h
```

-MD是将输出的结果保存到一个.d文件中

**-MM , -MMD**

同上，只不过，他不会考虑 #include<head_file> 指令，而只考虑 #include'head_file' 指令。