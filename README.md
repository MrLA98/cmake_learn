# CMake 学习笔记

## 0. 简单例子
c++ 代码：
```cpp
#include <iostream>
using namespace std;

int main() {
  cout << "hello!" << endl;
  return 0;
}
```
CMakeLists.txt 代码
```cmake
PROJECT(HELLO)

SET(SRC_LIST hello.cc)

message(STATUS "BINARY DIR: " ${HELLO_BINARY_DIR})

message(STATUS "SOURCE DIR: " ${HELLO_SOURCE_DIR})

add_executable(hello ${SRC_LIST})

```

在 CMakeLists.txt 所在的目录下，执行
```bash
cmake .
make
./hello
```
## 1. 基本语法介绍
### 1.1 PROJECT 关键字
> 写在最前面，只能有一个。

可以用来指定`工程名字`和`支持语言`
```cmake
PROJECT(HELLO) # 指定了工程名字 -- HELLO
PROJECT(HELLO CXX) # 指定了工程的名字，并且说明支持语言是 C++
PROJECT(HELLO C CXX) # 指定了工程的名字，并且说明支持语言是 C/C++
```

同时，该式子隐式定义了两个变量：
- `<project_name>_BINARY_DIR` -- 例子中是 HELLO_BINARY_DIR
- `<project_name>_SOURCE_DIR` -- 例子中是 HELLO_SOURCE_DIR
- 但这样写不好，建议使用 `PROJECT_BINARY_DIR` 和 `PROJECT_SOURCE_DIR`

### 1.2 SET 关键字
> 用来显式的指定变量

例子中的 `SET(SRC_LIST hello.cc)` 即为：定义了一个变量叫 `SRC_LIST`，这个变量的内容是 `hello.cc`

### 1.3 MESSAGE 关键字
> 向终端输出用户自定义的信息

三种信息：
- SEND_ERROR：产生错误，生成过程被跳过
- STATUS：输出前缀为 `-` 的信息
- FATAL_ERROR：立即终止所有 cmake 过程

### 1.4 ADD_EXECUTABLE 关键字
> 生成 `可执行文件`

第一个变量 -- 可执行文件名称
第二个变量 -- 可执行文件依赖的源文件列表

## 2 变量与参数

- 变量使用 `${}` 的方式取值，但在 IF 控制语句中直接使用变量名
- 指令(参数1 参数2;参数3...) 参数之间使用空格或 `;` 分割
- 指令是大小写无关的 -- 推荐使用大写指令
- 参数和变量是大小写有关的
- 注意⚠️：如果参数中有空格，参数应该用 `""` 括住，其余情况加不加引号无所谓

## 3 内部构建与外部构建
### 内部构建
在 CMakeLists.txt 目录下执行 `cmake .`
### 外部构建
在 CMakeLists.txt 目录下创建 `build` 目录，然后在 build 目录下执行 `cmake ..`

## 4 一个完整的工程
```bash
.
├── CMakeLists.txt
├── build
│   ├── Makefile
│   ├── bin # 可执行文件所在位置
│   └── ...
├── build.sh
├── runhello.sh
└── src
    ├── CMakeLists.txt 
    └── hello.cc
```
- 源文件所在目录要有 CMakeLists.txt
  - 只需要写“添加可执行文件”的部分即可
- 外层目录的 CMakeLists.txt
  - 需要写明工程名称
  - 需要添加子目录 -- 源文件所在目录


## 5 静态库和动态库

### 创建

- 动态库 -- libhello.so
  - add_library(hello SHARED hello.cc)

- 静态库 -- libhello.a
  - add_library(hello STATIC hello.cc)

- 需要同时生成动态库和静态库时：
```cmake
# 1. 两个库变量名不能相同
add_library(hello_static SHARED hello.cc)
# 2. 通过该方法来设置输出名字
set_target_properties(hello_static PROPERTIES OUTPUT_NAME "hello")
# 3. 特殊设置，防止二次生成出错
set_target_properties(hello_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)

add_library(hello STATIC hello.cc)
set_target_properties(hello PROPERTIES  OUTPUT_NAME "hello")
set_target_properties(hello PROPERTIES CLEAN_DIRECT_OUTPUT 1)
```

### 使用

```cmake
# 添加头文件搜索路径
include_directories(/usr/include/hello)
# 添加第三方库所在路径
link_directories(/home/mylibs/libs)
# 添加第三方库 -- 该库需要在标准的库搜索路径下
target_link_libraries(main libhello.so)
```

## 备注
```bash
# 编译成debug版本
cmake -DCMAKE_BUILD_TYPE=debug 
```
