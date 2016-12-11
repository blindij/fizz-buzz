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

include_directories(${PROJECT_SOURCE_DIR}/src)

add_library(
    fizz_buzz
    SHARED
    src/fizz_buzz.f90
    src/fizz_buzz.h
    src/divisible.f90
    )

add_executable(
    fb.x
    src/main.cpp
    )

target_link_libraries(
    fb.x
    fizz_buzz
    )

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/cmake)

include(tests)
