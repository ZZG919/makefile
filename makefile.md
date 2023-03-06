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

#### 版本1

```
###VERSION 1  如果改变了依赖文件，就全部编译
hello: main.cpp printhello.cpp factorial.cpp
	g++ -o hello main.cpp printhello.cpp factorial.cpp
```

#### 版本2

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

**patsubst** - 匹配替换
格式：$(patsubst,, )

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

