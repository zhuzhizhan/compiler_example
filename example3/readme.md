## 1 动态链接库

gcc -fPIC -shared hello.c -o libhello.so 

gcc main.c -L. -lhello -o main

sudo cp libhello.so /usr/lib/        //动态库链接时,会默认从/usr/lib查找.

## 2 静态链接库

gcc hello.c -o hello.o  #这里没有使用-shared

ar -r libhello.a hello.o  #这里的ar相当于tar的作用，将多个目标打包

gcc main.c -lhello -L. -static -o main  

gcc main.c libhello.a -L. -o main

## 3 makefile实例

### 3.1 静态库的生成

```
# 1、准备工作，编译方式、目标文件名、依赖库路径的定义。
CC = g++
CFLAGS  := -Wall -O3 -std=c++0x 

# opencv 头文件和lib路径 
OPENCV_INC_ROOT = /usr/local/include/opencv 
OPENCV_LIB_ROOT = /usr/local/lib

OBJS = GenDll.o #.o文件与.cpp文件同名
LIB = libgendll.a # 目标文件名 

OPENCV_INC= -I $(OPENCV_INC_ROOT)

INCLUDE_PATH = $(OPENCV_INC)

LIB_PATH = -L $(OPENCV_LIB_ROOT)

# 依赖的lib名称
OPENCV_LIB = -lopencv_objdetect -lopencv_core -lopencv_highgui -lopencv_imgproc

all : $(LIB)

# 2. 生成.o文件 
%.o : %.cpp
    $(CC) $(CFLAGS) -c $< -o $@ $(INCLUDE_PATH) $(LIB_PATH) $(OPENCV_LIB) 

# 3. 生成静态库文件
$(LIB) : $(OBJS)
    rm -f $@
    ar cr $@ $(OBJS)
    rm -f $(OBJS)

tags :
     ctags -R *

# 4. 删除中间过程生成的文件 
clean:
    rm -f $(OBJS) $(TARGET) $(LIB)
```

### 3.2 动态库的生成

```
# 1、准备工作，编译方式、目标文件名、依赖库路径的定义。
CC = g++
CFLAGS  := -Wall -O3 -std=c++0x 

# opencv 头文件和lib路径 
OPENCV_INC_ROOT = /usr/local/include/opencv 
OPENCV_LIB_ROOT = /usr/local/lib

OBJS = GenDll.o #.o文件与.cpp文件同名
LIB = libgendll.so # 目标文件名 

OPENCV_INC= -I $(OPENCV_INC_ROOT)

INCLUDE_PATH = $(OPENCV_INC)

LIB_PATH = -L $(OPENCV_LIB_ROOT)

# 依赖的lib名称
OPENCV_LIB = -lopencv_objdetect -lopencv_core -lopencv_highgui -lopencv_imgproc

all : $(LIB)

# 2. 生成.o文件 
%.o : %.cpp
    $(CC) $(CFLAGS) -fpic -c $< -o $@ $(INCLUDE_PATH) $(LIB_PATH) $(OPENCV_LIB) 

# 3. 生成动态库文件
$(LIB) : $(OBJS)
    rm -f $@
    g++ -shared -o $@ $(OBJS)
    rm -f $(OBJS)

tags :
     ctags -R *

# 4. 删除中间过程生成的文件 
clean:
    rm -f $(OBJS) $(TARGET) $(LIB)
```

### 3.3 动态库和静态库的调用

```
CC = g++
CFLAGS  := -Wall -O3 -std=c++0x 

OPENCV_INC_ROOT = /usr/local/include/opencv 
OPENCV_LIB_ROOT = /usr/local/lib
MY_ROOT = ../

OPENCV_INC= -I $(OPENCV_INC_ROOT)
MY_INC = -I $(MY_ROOT)

EXT_INC = $(OPENCV_INC) $(MY_INC)

OPENCV_LIB_PATH = -L $(OPENCV_LIB_ROOT)
MY_LIB_PATH = -L $(MY_ROOT)

EXT_LIB = $(OPENCV_LIB_PATH) $(MY_LIB_PATH) 

OPENCV_LIB_NAME = -lopencv_objdetect -lopencv_highgui -lopencv_imgproc -lopencv_core 
MY_LIB_NAME = -lgendll

all:test

test:main.cpp
    $(CC) $(CFLAGS) main.cpp $(EXT_INC) $(EXT_LIB) $(MY_LIB_NAME) $(OPENCV_LIB_NAME) -o test
```

## 4 注意

1、在测试过程中，经常会报错：找不到.so文件。一种简单的解决方法如下： 
在linux终端输入如下命令：

```plain
export LD_LIBRARY_PATH=/home/shaoxiaohu/lib:LD_LIBRARY_PATH:
```