cmake_minimum_required(VERSION 2.8) 
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(solver_lib STATIC solver.h solver.cpp)
