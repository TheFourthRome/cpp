cmake_minimum_required(VERSION 3.28.3)
project(test VERSION 0.1.0 LANGUAGES C CXX)
add_executable(test main.cpp)

# 设置C++标准(gnu++23 in gnu compiler)  
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)  
 
# 链接AddressSanitizer运行时库  
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    # --------------------------------------DEBUG------------------------------------------------------------------
    # 添加编译器标志来启用Sanitizers
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fsanitize=address,leak,undefined -fno-omit-frame-pointer")

    # # 添加编译器标志来启用Sanitizers-多线程环境
    # #set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fsanitize=thread -fno-omit-frame-pointer -g")

    # 添加编译器标志输出警告信息
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall -Wextra -pedantic -Werror")

    # 取消优化
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -O0")
    # --------------------------------------RELEASE------------------------------------------------------------------
    set(CMAKE_CXX_FLAGS_Release "${CMAKE_CXX_FLAGS_Release} -flto")
endif()
 
##其他

# 在Ubuntu上安装fmt库：sudo apt-get install libfmt-dev，然后才可使用下面的fmt库
# 对于其他系统，见https://fmt.dev/
# find_package(fmt REQUIRED)
# target_link_libraries(test fmt::fmt)

##对生成的Release版程序，可使用valgrind排查错误：
# valgrind --tool=memcheck --leak-check=yes --show-reachable=yes <path_to_excutable>

##记录程序运行过程，并使用gdb查看：rr（https://github.com/rr-debugger/rr）

##万能头文件 #include <bits/stdc++.h>

##常用库：proxy.h（https://github.com/microsoft/proxy）
##标准库扩展 boost、coost和abseil，工具库folly，异步框架async_simple（https://github.com/alibaba/async_simple）