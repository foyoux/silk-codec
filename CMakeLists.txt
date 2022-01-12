cmake_minimum_required(VERSION 3.21)
project(silk-codec VERSION 1.0.0)

# 设置编译类型，也可在CLION设置中设置
# set(CMAKE_BUILD_TYPE Release)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED True)

# silk
include_directories(interface)
include_directories(src)


add_executable(silk-encoder test/Encoder.c)
add_executable(silk-decoder test/Decoder.c)
add_executable(silk-signal-compare test/signalCompare.c)

add_subdirectory(src)

target_link_libraries(silk-encoder PUBLIC SKP_Silk_SDK)
target_link_libraries(silk-decoder PUBLIC SKP_Silk_SDK)
target_link_libraries(silk-signal-compare PUBLIC SKP_Silk_SDK)