# CMake入门笔记

## 运行环境
win10
cmake version 3.19.0-rc2
virtual studio 2019

## 1.使用CMake创建exe文件HelloWorld项目

在bash中运行cmake .命令，会根据当前目录下的CMakeLists.txt文件中的命令创建项目

### CMakeLists.txt文件内容

cmake_minimum_required (VERSION 3.1) #指定需求的最低CMake版本

project (HelloWorld) #project命名

add_executable (HelloWorld HelloWorld.cpp)  #使用HelloWorld.cpp创建名为HelloWorld的exe项目

### 运行效果
在根目录下生成了HelloWorld项目文件，使用vs编译后成功生成HelloWorld.exe

## 2.使用CMake创建dll库

在根目录下应该包含XXX.h、XXX.cpp和CMakeLists.txt，在XXX.h需要用到的函数类型后加__declspec(dllexport)标明需要将其包含在dll库中

### CMakeLists.txt文件内容
set(LIBMATH_SRC MathFunctions.cpp) # 设置简写名

add_library(Math SHARED ${LIBMATH_SRC}) #添加动态库

install(TARGETS Math RUNTIME DESTINATION D:/exercise/cmake_dll/lib) #安装动态库

SET_TARGET_PROPERTIES(Math PROPERTIES LINKER_LANGUAGE C) #设置linker
### 运行效果
在根目录Debug下生成了Math.dll

## 3.使用CMake创建使用dll库的exe项目

