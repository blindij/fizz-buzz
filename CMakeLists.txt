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

option(ENABLE_UNIT_TESTS "Enable unit tests" ON)
message(STATUS "Enable testing: ${ENABLE_UNIT_TESTS}")

if(ENABLE_UNIT_TESTS)
    include(ExternalProject)

    ExternalProject_Add(
        gtest
        PREFIX "${PROJECT_BINARY_DIR}/gtest"
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG master
        INSTALL_COMMAND true  # currently no install command
        )

    include_directories(${PROJECT_BINARY_DIR}/gtest/src/gtest/googletest/include)
    link_directories(${PROJECT_BINARY_DIR}/gtest/src/gtest-build/googlemock/gtest/)

    add_executable(
        unit_tests
        test/main.cpp
        test/fizz_buzz.cpp
        )

    target_link_libraries(
        unit_tests
        libgtest.a
        fizz_buzz
        pthread
        )

    add_dependencies(unit_tests gtest)

    include(CTest)
    enable_testing()

    add_test(unit ${PROJECT_BINARY_DIR}/bin/unit_tests)
endif()
