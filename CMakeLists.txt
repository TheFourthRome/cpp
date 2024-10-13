cmake_minimum_required(VERSION 3.28.3)
project(test VERSION 0.1.0 LANGUAGES C CXX)

# 查找C++编译器  
if(NOT CMAKE_CXX_COMPILER_ID MATCHES "GNU")  
    message(FATAL_ERROR "This project requires g++ to compile.")  
endif()  

# 设置C++标准(gnu++23 in gnu compiler)  
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED True)  
message(STATUS "Using C++23 standard.")

# 在Ubuntu上安装fmt库：sudo apt-get install libfmt-dev，然后才可使用下面的fmt库
# 对于其他系统，见https://fmt.dev/
add_executable(test main.cpp)
find_package(fmt REQUIRED)
target_link_libraries(test fmt::fmt)

cmake_minimum_required(VERSION 3.10)   

  
# 链接AddressSanitizer运行时库  
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU") 
    # 添加编译器标志来启用Sanitizers
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -fsanitize=address,leak,undefined -fno-omit-frame-pointer -g")

    ## 添加编译器标志来启用Sanitizers-多线程环境
    ##set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -fsanitize=thread -fno-omit-frame-pointer -g")

    # 添加编译器标志输出警告信息
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -Wall -Wextra -pedantic -Werror") 
        
    # 取消优化
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS} -O0")
    if(CMAKE_BUILD_TYPE MATCHES "Realease")   
        # 打印一些信息帮助调试 
        message(STATUS "Debug.")  
        message(STATUS "AddressSanitizer enabled.")  
        message(STATUS "Optimization disabled.")
    endif()
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS} -O3")
    if(CMAKE_BUILD_TYPE MATCHES "Realease")   
        # 打印一些信息帮助调试  
        message(STATUS "Release ver.")
    endif()   
endif() 

# 对生成的Release版程序，可使用valgrind排查错误：
# valgrind --tool=memcheck --leak-check=yes --show-reachable=yes <path_to_excutable>


#其他
##万能头文件 #include <bits/stdc++.h>