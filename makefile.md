### 多个源文件进行一次全部编译并且链接

```
g++ main.cpp factorial.cpp printhello.cpp -o main
./main 
```

### 编译单个文件不进行链接

 生成单个.o文件

```
g++ main.cpp -c
g++ factorial.cpp -c
g++ printhello.cpp -c
g++ *.o -o main
./main
```

## makefile

### make指令使用

```
###make会自动寻找当前目录下的makefile文件进行make
make
###或者指定makefile文件
make -f mymakefile
```

### makefile内容

#### makefile基本指令

```tex
目标文件名：多个依赖文件名以空格分开

\tab依赖文件变化后进行的指令操作
```

#### 版本1

```
###VERSION 1  如果改变了依赖文件，就全部编译
hello: main.cpp printhello.cpp factorial.cpp
	g++ -o hello main.cpp printhello.cpp factorial.cpp
```

#### 版本2

使用变量代替文本，使用$(变量名)获取遍历变量值。

```
### VERSION 2
CXX = g++
TARGET = hello 
OBJ = main.o printhello.o factorial.o

$(TARGET): $(OBJ)
	$(CXX) -o $(TARGET) $(OBJ)

main.o: main.cpp
	$(CXX) -c main.cpp
printhello.o: printhello.cpp
	$(CXX) -c printhello.cpp
factorial.o: factorial.cpp
	$(CXX) -c factorial.cpp
```

#### 版本3

使用'@'代替目标，使用'^'代替依赖，'%'为通配符，'<'为选取第一个。

```
### VERSION 2
CXX = g++
TARGET = hello 
OBJ = main.o printhello.o factorial.o

CXXFLAGS = -c -Wall

$(TARGET): $(OBJ)
	$(CXX) -o $@ $^
%.o = %.cpp
	$(CXX) $(CXXFLAGS) $< -O $@

.PHONY: clean
clean:
	rm -f *.o $(TARGET)
```

使用.PHONY的原因：

如果只有

```
clean:
	rm -f *.o $(TARGET)
```

那么在创建一个新的clean文件时，此时clean文件为最新文件，由于make clean指令是用来生成最新的clean文件，已经有最新文件了，那么make clean指令不执行

所以加上.PHONY: clean后，由于.PHONY文件不存在，所以.PHONY一直是最新的，会直接忽略clean对它的影响，所以执行make clean指令不受clean文件的影响。

#### 版本4

**wildcard** - 查找指定目录下的指定类型的文件。
展开为已经存在的、使用空格分开的、匹配此模式的所有文件列表。
SRC=$(wildcard *.cpp *.cc)

**patsubst** - 匹配替换格式：

$(patsubst <pattern>,<replacement>,<text> ) 

名称：模式字符串替换函数——patsubst。
功能：查找<text>中的单词（单词以“空格”、“Tab”或“回车”“换行”分隔）是否符合模式<pattern>，如果匹配的话，则以<replacement>替换。

　　　这里，<pattern>可以包括通配符“%”，表示任意长度的字串。如果<replacement>中也包含“%”，那么，<replacement>中的这个“%”将是<pattern>中的那个“%”所代表的字串。

　　　（可以用“\”来转义，以“\%”来表示真实含义的“%”字符）

​			返回：函数返回被替换过后的字符串。

示例：

$(patsubst %.c,%.o, a.c b.c)

把字串“a.c b.c”符合模式[%.c]的单词替换成[%.o]，返回结果是“a.o b.o”

```
### VERSION 2
CXX = g++
TARGET = hello
SRC = $(wildcard *.cpp)
OBJ = $(patsubst %.cpp,%.o,$(SRC))

CXXFLAGS = -c -Wall

$(TARGET): $(OBJ)
	$(CXX) -o $@ $^
%.o = %.cpp
	$(CXX) $(CXXFLAGS) $< -O $@

.PHONY: clean
clean:
	rm -f *.o $(TARGET)
```

