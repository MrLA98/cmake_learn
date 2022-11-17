PROJECT(HELLO)

SET(SRC_LIST hello.cc)

message(STATUS "BINARY DIR: " ${HELLO_BINARY_DIR})

message(STATUS "SOURCE DIR: " ${HELLO_SOURCE_DIR})

add_executable(hello ${SRC_LIST})

# 可以简写为：
# PROJECT(HELLO)
# add_executable(hello hello.cc)