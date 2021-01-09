cmake_minimum_required(VERSION 3.16)
project(hello)

add_executable(${PROJECT_NAME} main.cc)

message(${PROJECT_SOURCE_DIR})
include_directories(${PROJECT_SOURCE_DIR}/includes)
