cmake_minimum_required(VERSION 2.8.3)
project(ros_cuda_example)

find_package(CUDA REQUIRED)
set(
  CUDA_NVCC_FLAGS
  ${CUDA_NVCC_FLAGS};
  -O3
  -gencode arch=compute_30,code=sm_30
  -gencode arch=compute_35,code=sm_35
  -gencode arch=compute_50,code=[sm_50,compute_50]
  -gencode arch=compute_52,code=[sm_52,compute_52]
  -gencode arch=compute_61,code=sm_61
  -gencode arch=compute_62,code=sm_62
)

set(CUDA_VERBOSE_BUILD ON CACHE BOOL "nvcc verbose" FORCE)
set(LIB_TYPE STATIC) 
cuda_add_library(testLib src/test.cu)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
)

catkin_package(
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_executable(testNode src/main.cpp)
add_dependencies(testNode testLib)

target_link_libraries(testNode
  ${catkin_LIBRARIES}
  testLib
)