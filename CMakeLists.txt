cmake_minimum_required(VERSION 3.10)
project(DeepLearningTemplate)

# 设置C++标准
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 设置编译选项
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic")

# 设置 libtorch 路径
list(APPEND CMAKE_PREFIX_PATH "/path/to/libtorch")

# 查找 libtorch 包
find_package(Torch REQUIRED)

# 查找 fmt 包
find_package(fmt REQUIRED)

# 查找 sox 包
find_package(Sox REQUIRED)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# 包含头文件目录
include_directories(
    ${TORCH_INCLUDE_DIRS}
    ${FMT_INCLUDE_DIRS}
    ${SOX_INCLUDE_DIRS}
    include
)

# 添加源文件
file(GLOB_RECURSE CPP_SOURCES pp_src/*.cpp)

# 添加可执行文件
add_executable(deep_learning_template ${CPP_SOURCES})

# 链接库
target_link_libraries(deep_learning_template
    ${TORCH_LIBRARIES}
    fmt::fmt
    ${SOX_LIBRARIES}
)

# 设置编译选项
set_property(TARGET deep_learning_template PROPERTY CXX_STANDARD 20)

# 设置 RPATH 以便找到动态链接库
set_target_properties(deep_learning_template PROPERTIES
    INSTALL_RPATH_USE_LINK_PATH TRUE
    BUILD_RPATH_USE_ORIGIN TRUE
)

# 禁用前缀“lib”用于可执行文件
set_target_properties(deep_learning_template PROPERTIES PREFIX "")

# 显示 CMake 配置信息
message(STATUS "Using libtorch from: ${TORCH_INSTALL_PREFIX}")
message(STATUS "Using fmt from: ${FMT_INSTALL_PREFIX}")
message(STATUS "Using sox from: ${SOX_INSTALL_PREFIX}")
