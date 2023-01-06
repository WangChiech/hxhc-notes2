---
{"dg-publish":true,"permalink":"/2-areas/programming/cpp/c-make-lists-template/"}
---

## File tree
```
|-- .clang-format
|-- CMakeLists.txt
|-- build
|-- bin
    |-- release
        |-- a.exe
    |-- debug
        |-- a.exe
|-- include
    |-- ProjectName
        |-- header1.h
        |-- header2.hpp
|-- src
    |-- main.cpp
    |-- class1.cpp
```

## CMakeLists.txt template
```CMake
cmake_minimum_required(VERSION 3.12)
project(ToleranceInterval
    VERSION 0.0.1
    DESCRIPTION "A lib for test"
    LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 17)

file(GLOB_RECURSE source CONFIGURE_DEPENDS include/*.h include/*.hpp src/*.cpp)

find_package(Boost REQUIRED)
find_package(fmt REQUIRED)

set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_SOURCE_DIR}/bin/${CMAKE_BUILD_TYPE})

add_executable(${PROJECT_NAME} ${source})
if (MSVC)
    # warning level 4 and all warnings as errors
    target_compile_options(${PROJECT_NAME} PRIVATE /W4)
else()
    # lots of warnings and all warnings as errors
    target_compile_options(${PROJECT_NAME} PRIVATE -Wall -Wextra -Wpedantic)
endif()

target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)
# Without Boost::boost is also OK for header-only Boost libs
# using fmt::fmt-header-only for fmt header-only usage
target_link_libraries(${PROJECT_NAME} PRIVATE fmt::fmt Boost::boost)
```