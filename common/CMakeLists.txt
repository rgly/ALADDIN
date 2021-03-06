set(aladdin_main "aladdin.cpp")

FILE(GLOB ALADDIN_SRC "*.cpp")
list(REMOVE_ITEM ALADDIN_SRC ${aladdin_main})

set(CACTI_DIR "cacti-p")
FILE(GLOB CACTI_SRC "${CACTI_DIR}/*.cc")

if (NOT DEFINED NTHREADS)
  set(NTHREADS 8)
endif()

# find Boost library
include_directories(${ZLIB_INCLUDE_DIRS})

add_definitions("-DNTHREADS=${NTHREADS}")
include_directories("${CACTI_DIR}")

add_library(cacit-p ${CACTI_SRC})
add_library(aladdin_lib ${ALADDIN_SRC})
set_target_properties(aladdin_lib PROPERTIES OUTPUT_NAME "aladdin")


add_executable(aladdin ${aladdin_main})
target_link_libraries(aladdin aladdin_lib "cacit-p" ${Boost_LIBRARIES} pthread z)


if(${BUILD_ON_SOURCE})
  set_target_properties(aladdin PROPERTIES RUNTIME_OUTPUT_DIRECTORY
                         "${CMAKE_CURRENT_SOURCE_DIR}")
endif()
