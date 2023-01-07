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

## VSCode launch.json for debugging
>[!tip] 
> - Make sure that cmake build type is `debug` instead of `release`. While `RelWithDebInfo` also contains debug info, there are some optimizations that may disturb debugging. So I recommend use `debug`.
> - When using VSCode CMakeTools extension, I recommend using the clang kit for compiling, because it may also be used in Windows, Linux and MacOS. Indeed the Windows' clang toolchain is a wrapper of MSVC, so the ==C/C++== extension is recommend for debugging in Windows platform instead of ==CodeLLDB ==.
> - I don't recommend using ==Mingw== or ==Cygwin== just because I don't like them. Do whatever you like.
```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "vsdbg Launch",
            "type": "cppvsdbg",
            "request": "launch",
            "program": "${workspaceFolder}/bin/Debug/a.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceRoot}",
            "environment": [],
            "console": "integratedTerminal"
        },
        {
            "type": "lldb",
            "request": "launch",
            "name": "lldb Launch",
            "program": "${workspaceFolder}/bin/Debug/a.exe",
            "args": [],
            "cwd": "${workspaceFolder}"
        }
    ]
}
```