### cmake笔记
为了记录强大的cpp项目构建工具的知识点

### add_library()
add_library的作用是将指定的源文件生成链接文件，可以给外部使用，
一般需要将所有文件都链接出去,但是如果一个文件都要写的话，显然
非常麻烦，这里提供一种编程式过程自动化将所有文件添加到library
中。

***平凡的写法***
add_library(${PROJECT_NAME} src/main.cpp include/xx.h ...)

***编程自动化的写法***
```
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
```

```
foreach(dir ${SRC_DIRS})
# get directory sources and headers
    file(GLOB s_${dir} "${dir}/*.cpp")
    file(GLOB h_${dir} "${dir}/*.hpp")
    file(GLOB i_${dir} "${dir}/*.ipp")

# set sources
set(SOURCES ${SOURCES} ${s_${dir}} ${h_${dir}} ${i_${dir}})
endforeach()
```
这里的SOURCES变量是什么样子的呢，可以用`message(SOURCES)`打印出来
```
/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/builders/array_builder.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/builders/builders_factory.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/builders/bulk_string_builder.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/builders/error_builder.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/builders/integer_builder.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/builders/reply_builder.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/builders/simple_string_builder.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/core/client.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/core/reply.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/core/sentinel.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/core/subscriber.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/misc/logger.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/network/redis_connection.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/sources/network/tcp_client.cpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/array_builder.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/builder_iface.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/builders_factory.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/bulk_string_builder.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/error_builder.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/integer_builder.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/reply_builder.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/builders/simple_string_builder.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/core/client.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/core/reply.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/core/sentinel.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/core/subscriber.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/misc/error.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/misc/logger.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/misc/macro.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/network/redis_connection.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/network/tcp_client.hpp/
/home/nong/redis-demo/3rd_party/cpp_redis/includes/cpp_redis/network/tcp_client_iface.hpp
```

### 命令解释

***file命令*** 操作文件命令，
```
file(GLOB <variable>
        [LIST_DIRECTORIES true|false] [RELATIVE <path>] [CONFIGURE_DEPENDS]
        [<globbing-expressions>...])
file(GLOB_RECURSE <variable> [FOLLOW_SYMLINKS]
        [LIST_DIRECTORIES true|false] [RELATIVE <path>] [CONFIGURE_DEPENDS]
        [<globbing-expressions>...])
```
***例子1*** `file(GLOB s_${dir} "${dir}/*.cpp")`

```
Generate a list of files that match the <globbing-expressions> and store it into the <variable>. Globbing expressions are similar to regular expressions, but much simpler. If RELATIVE flag is specified, the results will be returned as relative paths to the given path. The results will be ordered lexicographically.
GLOB是一种表达式，例子1中的GLOG将表达式中生成文件列表并存储到变量中
```
***输出特定行数文本*** `sed -n 5,8p file`
