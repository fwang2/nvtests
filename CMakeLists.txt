cmake_minimum_required(VERSION 3.19)
project(nvtests LANGUAGES CXX CUDA)
if(NOT DEFINED CMAKE_CUDA_ARCHITECTURES)
  set(CMAKE_CUDA_ARCHITECTURES 70)
endif()


# c++ standard
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


find_package(MPI)
include_directories(SYSTEM ${MPI_INCLUDE_PATH})

find_package(CUDA 11.4 REQUIRED)

# set(BLA_VENDOR OpenBLAS)
# find_package(BLAS)

# add_definitions(-DEXTERNAL_CONV -DOTF_GEMV_OPTIMIZED -DNO_WARN_X86_INTRINSICS)
# add_compile_options(-W -Wall -fopenmp -mcpu=native)

set(nvlinkTest_SRCS 
    helper_cuda.h helper_timer.h helper_string.h p2pBandwidthLatencyTest.cu)

add_executable(nvlinkTest ${nvlinkTest_SRCS})
target_link_libraries(nvlinkTest
  ${MPI_LIBRARIES} 
  ${CUDA_LIBRARIES} 
)

set(bwTest_SRCS 
    helper_cuda.h helper_functions.h exception.h helper_image.h
    helper_string.h helper_timer.h bandwidthTest.cu)

add_executable(bwTest ${bwTest_SRCS})
target_link_libraries(bwTest
  ${MPI_LIBRARIES} 
  ${CUDA_LIBRARIES} 
)
