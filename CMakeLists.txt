cmake_minimum_required(VERSION 3.17)
project(VulkanDemo)

set(CMAKE_CXX_STANDARD 17)

# 检查环境变量
if (NOT DEFINED ENV{GLFW_HOME})
    message(FATAL_ERROR "found no env named GLFW_HOME")
endif()
if (NOT DEFINED ENV{GLM_HOME})
    message(FATAL_ERROR "found no env named GLM_HOME")
endif()

# 暂存环境变量
set(GLFW_HOME $ENV{GLFW_HOME})
set(GLM_HOME $ENV{GLM_HOME})
set(GLFW_INCLUDE_DIRS "${GLFW_HOME}/include")

add_executable(VulkanDemo main.cpp)

# 添加 GLFW3 预编译库
add_library(glfw SHARED IMPORTED)
SET_TARGET_PROPERTIES(glfw PROPERTIES IMPORTED_LOCATION "${GLFW_HOME}/lib/libglfw.3.dylib")

# GLM
include_directories(${GLM_INCLUDE_DIRS})

# GLFW
include_directories(${GLFW_INCLUDE_DIRS})

# Vulkan
find_package(Vulkan REQUIRED)
target_link_libraries(${PROJECT_NAME} glfw Vulkan::Vulkan)
#include_directories(${Vulkan_INCLUDE_DIRS})
#target_include_directories(${PROJECT_NAME} PUBLIC ${Vulkan_INCLUDE_DIRS})

# copy the shaders files to the cmake-build-debug folder
file(COPY shaders DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
