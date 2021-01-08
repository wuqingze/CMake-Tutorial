### cmake笔记
为了记录强大的cpp项目构建工具的知识点

### add_library()
写法
add_library(${PROJECT_NAME} src/main.cpp include/xx.h ...)
add_library的作用是将指定的源文件生成链接文件，可以给外部使用，
一般需要将所有文件都链接出去,但是如果一个文件都要写的话，显然
非常麻烦，这里提供一种编程式过程自动化将所有文件添加到library
中。
`
set(SRC_DIRS "sources"
        "sources/builders"
        "sources/core"
        "sources/misc"
        "sources/network"
        "includes/cpp_redis"
        "includes/cpp_redis/builders"
        "includes/cpp_redis/core"
        "includes/cpp_redis/misc"
        "includes/cpp_redis/network")
`

`
foreach(dir ${SRC_DIRS})
# get directory sources and headers
    file(GLOB s_${dir} "${dir}/*.cpp")
    file(GLOB h_${dir} "${dir}/*.hpp")
    file(GLOB i_${dir} "${dir}/*.ipp")

# set sources
set(SOURCES ${SOURCES} ${s_${dir}} ${h_${dir}} ${i_${dir}})
endforeach()
`

