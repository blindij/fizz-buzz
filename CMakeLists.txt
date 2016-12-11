cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(fizz-buzz)

enable_language(C)
enable_language(CXX)
enable_language(Fortran)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY
    ${CMAKE_BINARY_DIR}/bin
    )

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY
    ${CMAKE_BINARY_DIR}/lib
    )

set(CMAKE_Fortran_MODULE_DIRECTORY ${PROJECT_BINARY_DIR}/include/fortran)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/cmake)

include_directories(${PROJECT_SOURCE_DIR}/src)

add_subdirectory(src)

include(tests)
