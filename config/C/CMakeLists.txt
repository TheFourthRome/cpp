cmake_minimum_required(VERSION 3.28.3)
project(test VERSION 0.1.0 LANGUAGES C CXX)
add_executable(test main.c)

# 设置C++标准(gnu++23 in gnu compiler)  
set(CMAKE_C_STANDARD 2B)
set(CMAKE_C_STANDARD_REQUIRED True)  
 
# 链接AddressSanitizer运行时库  
if(CMAKE_C_COMPILER_ID MATCHES "GNU")
    # --------------------------------------DEBUG------------------------------------------------------------------
    # 添加编译器标志来启用Sanitizers
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -fsanitize=address,leak,undefined -fno-omit-frame-pointer")

    # # 添加编译器标志来启用Sanitizers-多线程环境
    # #set(CMAKE_C_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fsanitize=thread -fno-omit-frame-pointer -g")

    # 添加编译器标志输出警告信息
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -Wall -Wextra -pedantic -Werror")

    # 取消优化
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -O0")
    # --------------------------------------RELEASE------------------------------------------------------------------
    set(CMAKE_C_FLAGS_Release "${CMAKE_C_FLAGS_Release} -flto")
endif()
 
##其他

##debug工具：
# mtrace() 函数，mprotect()函数
##对生成的Release版程序，可使用valgrind排查错误：
# valgrind --tool=memcheck --leak-check=yes --show-reachable=yes <path_to_excutable>

##记录程序运行过程，并使用gdb查看：rr（https://github.com/rr-debugger/rr）