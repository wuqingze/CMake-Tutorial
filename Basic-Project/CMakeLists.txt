cmake_minimum_required(VERSION 3.5)
project(Basic-Project)

#include_directories(
#    ${PROJECT_SOURCE_DIR}/include
#)

add_executable(${PROJECT_NAME} main.cc)
target_include_directories( ${PROJECT_NAME}
    PUBLIC 
        ${PROJECT_SOURCE_DIR}/include
)
