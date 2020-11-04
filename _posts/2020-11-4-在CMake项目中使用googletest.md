googletest是谷歌开源的实用单元测试框架。

## 一.创建一个包含googletest的项目
![_config.yml]({{ site.baseurl }}/images/googletest.PNG)

生成的解决方案中包括两个exe项目和一个dll项目，以达到测试和开发分开的目的。根目录的main.cpp生成主程序，boolExprTest下的main.cpp生成测试程序。
&emsp;
&emsp;
### CMakeLists编写方法
### 1.根目录CMakeLists.txt
cmake_minimum_required(VERSION 3.10)

project (HelloGTest)

set(PROJECT_SOURCE_DIR D:/exercise/gtest)

**set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build)** #库文件输出目录

**set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build)** #设置执行文件输出目录

**add_subdirectory(boolExpr)** #引入函数库

**add_subdirectory(external/googletest)** #引入gtest库

**add_subdirectory(boolExprTest)** #引入test库

**add_executable(HelloGTest main.cpp)** #生成exe

**target_link_libraries( HelloGTest **
**&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; PRIVATE  **
**&emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; boolExpr) **

### 2.boolExpr下的CMakeLists.txt

set(BUILD_USE_64BITS on) #64位编程

project(boolExpr)

set(LIBEXPR_DLL boolExpr.cpp)

add_library(boolExpr SHARED ${LIBEXPR_DLL})

set_target_properties(boolExpr PROPERTIES LINKER_LANGUAGE C)

**与前文dll库项目相同**

### 3.boolExprTest下的CMakeLists.txt
project(boolExprTest)

**set(EXTERNALPATH ${CMAKE_CURRENT_SOURCE_DIR}../external)** #为外部库路径起别名
**set(BOOL_EXPR_ROOT ${CMAKE_CURRENT_SOURCE_DIR}../boolexpr)** #为boolExpr库路径起别名
**set(BOOL_EXPR_TESET ${CMAKE_CURRENT_SOURCE_DIR})** #为测试库路径起别名

**add_executable(boolExprTest boolExprTest.cpp main.cpp)** #使用main.cpp和boolExprTest.cpp生成测试程序，测试代码较多时可以使用**file（）**命令为当前目录下的所有cpp文件定义一个变量名。**main.cpp**可以参考googletest的使用样例编写。

**set_target_properties(boolExprTest PROPERTIES FOLDER "GTest")**
**target_link_libraries(boolExprTest boolExpr gmock gtest)**

**target_include_directories(
&emsp;boolExpr
&emsp;PRIVATE &nbsp; ${EXTERNALPATH}/googletest/googletest/include
&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp; &nbsp; ${EXTERNALPATH}/googletest/googlemock/include
&emsp;&emsp;&emsp;&emsp;&emsp;&nbsp; &nbsp; ${BOOL_EXPR_ROOT})** #将googletest的头文件和待测试头文件include进来

**target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)** 指定编译c++版本

### 在根目录运行cmake .，就可以成功创建所需要的项目。
编译结果：
![_config.yml]({{ site.baseurl }}/images/TESTEXE.png)
&emsp;
&emsp;
### 二.编写XXXtest.cpp

在googletest中，比较常用的测试宏是TEST、TEST_F、TEST_P，较常用的断言是EXPECT和ASSERT

### TEST宏
TEST宏用法：**TEST(待测试函数名，测试名){测试内容}**&nbsp; &nbsp;测试名可以自拟，最好起名与测试内容相关
EXPECT断言在遇到失败测试之后会继续将所有测试用例运行完，ASSERT断言在测试失败后会退出、一般用于较关键功能的测试。
示例：
![_config.yml]({{ site.baseurl }}/images/TEST.PNG)

运行效果：
![_config.yml]({{ site.baseurl }}/images/TESTEXERUN.png) 这里非运算函数故意写错了，也在测试中有反馈

### TEST_F宏
TEST_F宏用于将同一个测试用例应用于多个测试。
在使用TEST_F时，必须先声明一个类，显式继承自待测试类和testing::Test类，并在其中声明SetUpTestCase()方法（在第一个TEST_F前执行，用于构造类）和TearDownTestCase()方法（在最后一个TEST后执行，用于析构）。

示例代码：
![_config.yml]({{ site.baseurl }}/images/TEST_F.png)

### TEST_P宏
//TODO 
