cmake_minimum_required(VERSION 2.8) 
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
add_library(formatter STATIC formatter.h formatter.cpp)
