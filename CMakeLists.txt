cmake_minimum_required(VERSION 3.24)
project(algebra_assignment VERSION 0.1.0 LANGUAGES CXX)

set(ALGEBRA_ASSIGNMENT_SIZE 4000 CACHE STRING "Square matrix dimension")
if(NOT ALGEBRA_ASSIGNMENT_SIZE MATCHES "^[1-9][0-9]*$")
    message(FATAL_ERROR "The matrix dimension must be a positive integer")
endif()

include(cmake/Dependencies.cmake)
add_subdirectory(src)
