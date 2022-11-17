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