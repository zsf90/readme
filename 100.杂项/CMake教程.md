## 设置CMake最低版本

```cmake
cmake_minimum_required(VERSION 3.4.1)
```

在有些情况下，如果 `CMakeLists.txt` 文件中使用了一些高版本 `cmake` 特有的一些命令的时候，就需要加上这样一行，提醒用户升级到该版本之后再执行 `cmake`。

## 设置项目名称

```cmake
project(demo)
```

最好写上，它会引入两个变量 `demo_BINARY_DIR` 和 `demo_SOURCE_DIR`，同时，`cmake` 自动定义了两个等价的变量 `PROJECT_BINARY_DIR` 和 `PROJECT_SOURCE_DIR`。

## 设置编译类型

```cmake
add_executable(demo demo.cpp) # 生成可执行文件
add_library(common STATIC util.cpp) # 生成静态库
add_library(common SHARED util.cpp) # 生成动态库或共享库
```

**add_library** 默认生成是静态库，通过以上命令生成文件名字，

在 **Linux** 下是： demo libcommon.a libcommon.so

在 **Windows** 下是：demo.exe common.lib common.dll

## 指定编译包含的源文件

1. 明确支出包含那些源文件

```cmake
add_library(demo demo.cpp test.cpp util.cpp)
```

2. 搜索所有的 CPP 文件

**aux_source_directory(dir VAR)** 发现一个目录（dir）下所有的源代码文件并将列表存储在房水一个变量（VAR）中。

```cmake
aux_source_directory(. SRC_LIST) # 搜索当前目录下的所有 .cpp 文件
add_library(demo ${SRC_LIST})
```



3. 自定义搜索规则

```cmake
file(GLOB SRC_LIST "*.cpp" "protocol/*.cpp")
add_library(demo ${SRC_LIST})
# 或者
file(GLOB SRC_LIST "*.cpp")
file(GLOB SRC_PROTOCOL_LIST "protocol/*.cpp")
add_library(demo ${SRC_LIST} ${SRC_PROTOCOL_LIST})
# 或者
aux_source_directory(. SRC_LIST)
aux_source_directory(protocol SRC_PROTOCOL_LIST)
add_library(demo ${SRC_LIST} ${SRC_PROTOCOL_LIST})
```

## 查找指定的库文件

**find_library(VAR name path)** 查找到指定的预编译库，并将它的路径存储在变量中。

默认的搜索路径为 **cmake** 包含的系统库，因此如果是 NDK 的公共库只需要指定库的 name 即可（不需path）

```cmake
find_library(log-lib, log)
```

类似的命令还有 **find_file()**、**find_path()**、**find_program()**、**find_package()**。

## 设置包含的目录

```cmake
include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}
	${CMAKE_CURRENT_BINARY_DIR}
	${CMAKE_CURRENT_SOURCE_DIR}/include
)
```

## 设置 target 需要链接的库

```cmake
target_link_libraries( # 目标库 demo # 目标库需要链接的库 ${log-lib})
```

在 Windows 下，系统会根据链接库目录，搜索 xxx.lib文件，Linux 下会搜索 xxx.so 或者 xxx.a 文件，如果都存在会优先链接动态库（so 后缀）。

1. 指定链接动态库或静态库

```cmake
target_link_libraries(demo libface.a) # 链接 libface.a
target_list_libraries(demo libface.so) # 链接 libface.so
```

2. 指定全路径

```cmake
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a)
target_link_libraries(demo ${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.so)
```

3. 指定多个链接库

```cmake
target_link_libraries(demo
	${CMAKE_CURRENT_SOURCE_DIR}/libs/libface.a
	boost_system.a
	boost_thread
	pthread
)
```

## 设置变量

1. set 直接设置变量的值

```cmake
set(SRC_LIST main.cpp test.cpp)
add_executable(demo ${SRC_LIST})
```

2. set 追加设置变量的值

```cmake
set(SRC_LIST main.cpp)
set(SRC_LIST ${SRC_LIST} test.cpp)
add_executable(demo ${SRC_LIST})
```

## 常用变量

**PROJECT_SOURCE_DIR**: 工程的根目录

**PROJECT_BINARY_DIR**: 运行cmake命令的目录，通常为 **${PROJECT_SOURCE_DIR}/build**

**PROJECT_NAME**: 返回通过 project 命令定义的项目名称

**CMAKE_CURRENT_SOURCE_DIR**: 当前处理的 **CMakeList.txt** 所在的路径

**CMAKE_CURRENT_BINARY_DIR**：**target** 编译目录

**CMAKE_CURRENT_LIST_DIR**: **CMakeLists.txt** 的完整路径

**EXECUTABLE_OUTPUT_PATH**: 重新定义目标二进制可执行文件的存放位置

**LIBRARY_OUTPUT_PATH**: 重新定义目标链接库文件的存放位置



## 添加头文件目录、链接动态、静态库

### include_directories 添加头文件目录

语法：

include_directories([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])

它相当于**g++** 选项中的 **-I** 参数的作用，也相当于环境变量中增加路径到**CPLUS_INCLUDE_PATH** 变量的作用。

```include_directories(./inc)```

### link_directories 添加需要链接的库文件目录

语法

```link_directories(directory1 directory2 ...)```

它相当于 **g++** 命令的 **-L** 选项的作用，也相当于环境变量中增加 **LD_LIBRARY_PATH** 的路径的作用。

```link_directories("/home/server/third/lib")```

### find_library 查找库所在目录

语法

```shell
A short-hand signature is:

find_library (<VAR> name1 [path1 path2 ...])
The general signature is:

find_library (
          <VAR>
          name | NAMES name1 [name2 ...] [NAMES_PER_DIR]
          [HINTS path1 [path2 ... ENV var]]
          [PATHS path1 [path2 ... ENV var]]
          [PATH_SUFFIXES suffix1 [suffix2 ...]]
          [DOC "cache documentation string"]
          [NO_DEFAULT_PATH]
          [NO_CMAKE_ENVIRONMENT_PATH]
          [NO_CMAKE_PATH]
          [NO_SYSTEM_ENVIRONMENT_PATH]
          [NO_CMAKE_SYSTEM_PATH]
          [CMAKE_FIND_ROOT_PATH_BOTH |
           ONLY_CMAKE_FIND_ROOT_PATH |
           NO_CMAKE_FIND_ROOT_PATH]
         )
```

例子如下：

```cmake
FIND_LIBRARY(RUNTIME_LIB rt /usr/lib  /usr/local/lib NO_DEFAULT_PATH)
```

**cmake** 会在目录中查找，如果所有目录中都没有，值 **RUNTIME_LIB** 就会被赋为 **NO_DEFAULT_PATH**

### link_libraries 添加需要链接的库文件路径

语法：

```link_libraries(library1 <debug | optimized> library2 ...)```

```cmake
# 直接是全路径
link_libraries(“/home/server/third/lib/libcommon.a”)
# 下面的例子，只有库名，cmake会自动去所包含的目录搜索
link_libraries(iconv)
# 传入变量
link_libraries(${RUNTIME_LIB})
# 也可以链接多个
link_libraries("/opt/MATLAB/R2012a/bin/glnxa64/libeng.so"　"/opt/MATLAB/R2012a/bin/glnxa64/libmx.so")
```

可以链接一个，也可以多个，中间使用空格分隔.

### target_link_libraries 设置要链接的库文件的名称

语法：

target_link_libraries(<target> [item1 [item2 [...]]]
                      [[debug|optimized|general] <item>] ...)

```cmake
# 以下写法都可以： 
target_link_libraries(myProject comm)       # 连接libhello.so库，默认优先链接动态库
target_link_libraries(myProject libcomm.a)  # 显示指定链接静态库
target_link_libraries(myProject libcomm.so) # 显示指定链接动态库

# 再如：
target_link_libraries(myProject libcomm.so)　　#这些库名写法都可以。
target_link_libraries(myProject comm)
target_link_libraries(myProject -lcomm)
```

### 一个例子

```cmake
cmake_minimum_required (VERSION 2.6)

INCLUDE_DIRECTORIES(../../thirdparty/comm)

FIND_LIBRARY(COMM_LIB comm ../../thirdparty/comm/lib NO_DEFAULT_PATH)
FIND_LIBRARY(RUNTIME_LIB rt /usr/lib  /usr/local/lib NO_DEFAULT_PATH)

link_libraries(${COMM_LIB} ${RUNTIME_LIB})

ADD_DEFINITIONS(
-O3 -g -W -Wall
 -Wunused-variable -Wunused-parameter -Wunused-function -Wunused
 -Wno-deprecated -Woverloaded-virtual -Wwrite-strings
 -D__WUR= -D_REENTRANT -D_FILE_OFFSET_BITS=64 -DTIXML_USE_STL
)


add_library(lib_demo
        cmd.cpp
        global.cpp
        md5.cpp
)

link_libraries(lib_demo)
add_executable(demo
        main.cpp
)

# link library in static mode
target_link_libraries(demo libuuid.a)
```

## 设置宏定义

```cmake
# 预定义宏
add_definitions(-D宏名称)
```

例如：

```cmake
add_definitions(-DWINDOWS)
add_definitions(-DLINUX)
```

## file 语法

将文件夹所有的类型的文件添加到文件列表

例如将当前文件夹下所有.cpp文件的文件名加入到MAIN_SRC中，将当前文件夹下所有.h加入到MAIN_HDR中。

```cmake
file(GLOB MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
file(GLOB MAIN_HDR ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
```

递归搜索该文件夹，将文件夹下（包含子目录）符合类型的文件添加到文件列表

例如将当前文件夹下（包括子目录下）所有.cpp文件的文件名加入到MAIN_SRC中，所有.h加入到MAIN_HDR中

```cmake
file(GLOB_RECURSE MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
file(GLOB_RECURSE MAIN_HDR ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
```

## List 操作

常见的List操作包括：

```cmake
list(LENGTH <list> <output variable>) 
list(GET <list> <element index> [<element index> ...]
   <output variable>)
list(APPEND <list> [<element> ...])
list(FIND <list> <value> <output variable>)
list(INSERT <list> <element_index> <element> [<element> ...])
list(REMOVE_ITEM <list> <value> [<value> ...])
list(REMOVE_AT <list> <index> [<index> ...])
list(REMOVE_DUPLICATES <list>)
list(REVERSE <list>)
list(SORT <list>)

```

###  List 移除指定项

例如从 MAIN_SRC 移除指定项

```cmake
list(REMOVE_ITEM MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/add.cpp)
```

### 将两个 List 链接起来

```cmake
# 搜索当前目录
file(GLOB  MAIN_SRC ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
file(GLOB  MAIN_HDR ${CMAKE_CURRENT_SOURCE_DIR}/*.h)

# 递归搜索当前目录下src子目录
file(GLOB_RECURSE MAIN_SRC_ELSE  ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)
file(GLOB_RECURSE MAIN_HDR_ELSE  ${CMAKE_CURRENT_SOURCE_DIR}/src/*.h)

# 将MAIN_SRC_ELSE中的值添加到MAIN_SRC 
# 将MAIN_HDR_ELSE中的值添加到MAIN_HDR 
list(APPEND MAIN_SRC ${MAIN_SRC_ELSE})
list(APPEND MAIN_HDR ${MAIN_HDR_ELSE})
```

## message 输出消息机制

### 正常输出

```cmake
message(STATUS "Enter cmake ${CMAKE_CURRENT_LIST_DIR}")
```

### 警告输出

```cmake
message(WARNING "Enter cmake ${CMAKE_CURRENT_LIST_DIR}")
```

### 输出错误

```cmake
message(FATAL_ERROR "Enter cmake ${CMAKE_CURRENT_LIST_DIR}")
```

## 安装 install

install 指令用于定义安装规则，安装的内容包括二进制可执行文件、动态库、静态库以及文件、目录、脚本等。

### 目标文件安装

例如：

```cmake
install(TARGETS util
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib)
```

ARCHIVE指静态库，LIBRARY指动态库，RUNTIME指可执行目标二进制，上述示例的意思是：

如果目标util是可执行二进制目标，则安装到${CMAKE_INSTALL_PREFIX}/bin目录 如果目标util是静态库，则安装到安装到${CMAKE_INSTALL_PREFIX}/lib目录 如果目标util是动态库，则安装到安装到${CMAKE_INSTALL_PREFIX}/lib目录

### 文件夹安装

```cmake
install(DIRECTORY include/ DESTINATION include/util)
```

这个语句的意思是将include/目录安装到include/util目录

### 设置编译选项

置编译选项可以通过add_compile_options命令，也可以通过set命令修改CMAKE_CXX_FLAGS或CMAKE_C_FLAGS。 方式1

```cmake
set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -march=native -O3 -frtti -fpermissive -fexceptions -pthread")
```

方式2：

```cmake
add_compile_options(-march=native -O3 -fexceptions -pthread -fPIC)
```

这两种方式的区别在于：

add_compile_options命令添加的编译选项是针对所有编译器的(包括c和c++编译器)，而set命令设置CMAKE_C_FLAGS或CMAKE_CXX_FLAGS变量则是分别只针对c和c++编译器的。

## 操作系统变量

CMAKE_MAJOR_VERSION cmake 主版本号

CMAKE_MINOR_VERSION cmake 次版本号

CMAKE_PATCH_VERSION cmake 补丁等级

CMAKE_SYSTEM 操作系统名称，包括版本号

CMAKE_SYSTEM_NAME 操作系统名称，不包括版本号

CMAKE_SYSTEM_VERSION 操作系统版本号

CMAKE_SYSTEM_PROCESSOR 电脑处理器名称

UNIX 所有的类 UNIX平台

WIN32 所有的Win32 平台

APPLE 苹果系统

判断操作系统的例子：

```cmake
if(WIN32)
    message(STATUS “This operating system is Windows.”)
elseif(UNIX)
    message(STATUS “This operating system is Linux.”)
elseif(APPLE)
    message(STATUS “This operating system is APPLE.”)
endif(WIN32)
```

方式2：

```cmake
if (CMAKE_SYSTEM_NAME MATCHES "Linux")
    message(STATUS "current platform: Linux ")
elseif (CMAKE_SYSTEM_NAME MATCHES "Windows")
    message(STATUS "current platform: Windows")
elseif (CMAKE_SYSTEM_NAME MATCHES "FreeBSD")
    message(STATUS "current platform: FreeBSD")
else ()
    message(STATUS "other platform: ${CMAKE_SYSTEM_NAME}")
endif (CMAKE_SYSTEM_NAME MATCHES "Linux")
```



## 开关选项

**BUILD_SHARED_LIBS** 控制默认的库编译方式。如果未进行设置,使用ADD_LIBRARY时又没有指定库类型,默认编译生成的库都是静态库；

**CMAKE_C_FLAGS** 设置C编译选项，也可以通过指令 ADD_DEFINITIONS() 添加

**CMAKE_CXX_FLAGS** 设置C++编译选项

**CMAKE_C_COMPILER** 指定C编译器

**CMAKE_CXX_COMPILER** 指定C++编译器

**CMAKE_BUILD_TYPE** Debug, Release

## 环境变量

设置环境变量

```cmake
set(env{name} value)
```

调用环境变量：

```cmake
$env{name}
```

例如：

```cmake
message(STATUS "$env{name}")
```

## CMAKE_INCLUDE_CURRENT_DIR

自动添加CMAKE_CURRENT_BINARY_DIR和CMAKE_CURRENT_SOURCE_DIR到当前处理 的CMakeLists.txt。 相当于在每个CMakeLists.txt加入：

```cmake
include_directories(${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_CURRENT_SOURCE_DIR})
```

## 条件判断

- ==if (expression)==：expression 不为空时为真，false的值包括（0,N,NO,OFF,FALSE,NOTFOUND）；
- ==if (not exp)==：与上面相反；
- ==if (var1 AND var2)==：如果两个变量都为真时为真；
- ==if (var1 OR var2)==：如果两个变量有一个为真时为真；
- ==if (COMMAND cmd)==：如果 cmd 确实是命令并可调用为真；
- ==if (EXISTS dir) if (EXISTS file)==：如果目录或文件存在为真；
- ==if (file1 IS_NEWER_THAN file2)==：当 file1 比 file2 新，或 file1/file2 中有一个不存在时为真，文件名需使用全路径；
- ==if (IS_DIRECTORY dir)==：当 dir 是目录时为真；
- ==if (DEFINED var)==：如果变量被定义为真；
- ==if (var MATCHES regex)==：给定的变量或者字符串能够匹配正则表达式 regex 时为真，此处 var 可以用 var 名，也可以用 ${var}；
- ==if (string MATCHES regex)==：给定的字符串能够匹配正则表达式regex时为真。

### 数字比较

- ==if (variable LESS number)==：如果variable小于number时为真；
- ==if (string LESS number)==：如果string小于number时为真；
- ==if (variable GREATER number)==：如果variable大于number时为真；
- ==if (string GREATER number)==：如果string大于number时为真；
- ==if (variable EQUAL number)==：如果variable等于number时为真；
- ==if (string EQUAL number)==：如果string等于number时为真。

### 字母表顺序比较

- ==if (variable STRLESS string)==
- ==if (string STRLESS string)==
- ==if (variable STRGREATER string)==
- ==if (string STRGREATER string)==
- ==if (variable STREQUAL string)==
- ==if (string STREQUAL string)==

## 循环

### foreach

start 表示起始数，stop 表示终止数，step 表示步长

```cmake
foreach(loop_var RANGE start stop [step])
    ...
endforeach(loop_var)
```

### while

```cmake
while(condition)
    ...
endwhile()
```

## 自动检测编译器是否支持C++11

```cmake
include(CheckCXXCompilerFlag)
CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

```



## 单文件例子

```cmake
cmake_minimum_required(VERSION 3.4.1)

project(demo)

add_executable(${PROJECT_NAME} src/main.c)
```

## 多文件例子

```cmake
cmake_minimum_required(VERSION 3.4.1)

project(demo)

add_executable(${PROJECT_NAME} src/main.c src/fun.c)
```

这个例子只是在上一个例子的基础上增加了 'src/fun.c' 文件，更好的做法是让cmake 自动查找指定目录下的所有源文件。

```aux_source_directory(<dir> <variable>)```

新的 CMakeList.txt

```cmake
cmake_minimum_required(VERSION 3.4.1)

project(demo)

# 把当前目录下的所有源文件保存到变量 DIR_SRCS 中
aux_source_directory(. DIR_SRCS)

add_executable(${PROJECT_NAME} ${DIR_SRCS})
```



