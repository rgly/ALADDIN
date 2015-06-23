cmake_minimum_required(VERSION 2.8.12)

# disable in-source-build
get_filename_component(srcdir "${CMAKE_SOURCE_DIR}" REALPATH)
get_filename_component(bindir "${CMAKE_BINARY_DIR}" REALPATH)
if("${srcdir}" STREQUAL "${bindir}")
  message(FATAL_ERROR "In-source builds are forbidden!")
endif()

project(Aladdin CXX)
enable_testing()

# Check whether LLVM-Tracer exists
set(TRACER_DIR "${CMAKE_SOURCE_DIR}/LLVM-Tracer")
if (NOT EXISTS ${TRACER_DIR})
  message(FATAL_ERROR "finds no ${TRACER_DIR}")
endif()

# settings for LLVM-Tracer
set(BUILD_ON_SOURCE FALSE)
set(HAS_ALADDIN TRUE)

# reuse cmake-script of LLVM-Tracer
set(SCRIPT_DIR "${TRACER_DIR}/cmake-scripts")
include("${SCRIPT_DIR}/TracerConfig.cmake")
include("${SCRIPT_DIR}/findAndSetLLVM.cmake")

# build LLVM-Tracer
add_subdirectory(${TRACER_DIR})

# build aladdin
add_subdirectory(common)
add_subdirectory(SHOC)