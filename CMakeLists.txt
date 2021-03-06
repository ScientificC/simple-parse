cmake_minimum_required(VERSION 2.6)
project(SParse)


set(SPARSE_MAJOR_VERSION 0)
set(SPARSE_MINOR_VERSION 1)
set(SPARSE_PATCH_VERSION 0)
set(SPARSE_VERSION
${SPARSE_MAJOR_VERSION}.${SPARSE_MINOR_VERSION}.${SPARSE_PATCH_VERSION})

INCLUDE(./cmake/CMakeBuildFlags.cmake)

# library
include_directories(./src)

# test
add_executable(sparse_test sparse_test.c src/sparse.c src/sparse.h)
enable_testing()
add_test(NAME TBool     COMMAND sparse_test --type=boolean --val=TRUE)
add_test(NAME TEmpty    COMMAND sparse_test --type=empty   --val=)
add_test(NAME TValue    COMMAND sparse_test --type=value   --val=innerValue)
add_test(NAME TMissing  COMMAND sparse_test --type=missing)
if (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    add_test (NAME MemCheck  COMMAND valgrind --error-exitcode=1 --tool=memcheck --leak-check=full --track-origins=yes -v ./sparse_example)
endif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")


set(SPARSE_INCLUDES ${CMAKE_CURRENT_SOURCE_DIR} CACHE STRING "sparse includes")

# example
add_executable(sparse_example sparse_example.c src/sparse.c src/sparse.h)
