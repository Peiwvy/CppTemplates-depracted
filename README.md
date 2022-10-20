# C++编程模版

这是一个C++编程模版，用于提高程序开发与调试效率。本部分与vscode结合紧密，强烈建议大家使用vscode作为首选IDE。

- [C++编程模版](#C++编程模版)
- [背景](#背景)
- [愿景](#愿景)
- [插件选择](#插件选择)
- [使用注意事项](#使用注意事项)
    - [moduels编写](#moduels编写)
    - [Doxygen编写](#Doxygen编写)
    - [命名规范](#命名规范)
    - [注释规范](#注释规范)
    - [结构划分](#结构划分)
    - [提交](#提交)
    - [依赖规范](#依赖规范)
- [TODO](#TODO)

# latest 

请大家参考[链接](https://sjtu.feishu.cn/docx/GNrBdbXlwop1ruxKjkhcFsb1nuv)

# How to use it

1. mkdir buld && cd build
2. cmake .. && make -j32

# 背景

在以往的开发中，您可能遇到下面一种或几种情况，这些问题阻塞了C++开发与调试工作的有序进行。

调试侧：
1. 由于gdb复杂，一直在使用 **cout大法** 进行调试，然而效率低下；
2. 尝试查询如何使用vscode自带debug工具，进行标准C++程序debug，但苦于每次debug必先修改配置文件，操作复杂；

ROS侧：
3. 因为ros使用catkin_make机制进行编译，其文件夹结构和标准C++程序结构存在差异，与vscode自带debug工具兼容度较低，导致难以debug ros相关程序；

程序结构侧：
4. cmakelist文件混乱，掌握颗粒度不够精细，可能在迁移编译时总是出现链接问题，教训效率低下；
5. 程序写在一个或少数几个庞大的node节点中，非模块化设计导致程序呈现低内聚高耦合的特性，想要进行二次开发和功能复用十分复杂；（您是否体会过对于自己编写程序，新增需求或者修改功能时的无赖？）

二次开发侧：
6. 没有明确的文档规范，导致工作移交过程存在困难，新维护人员需要花费更多精力阅读程序；
7. 没有单元测试功能，无法明确测试子模块可靠性；
8. 就算有单元测试，但是没法进行例子级别的单个测试案例运行与debug，于是测试案例必须全部同时运行的情况，造成不必要性能损失，调试效率低下；
9. 有效程序利用率低，缺少公共算法库，每个人都在重复造轮子（例如，kf），费时费力，也没有质量保证；
10. 缺少程序版本管理

# 愿景

我们希望构建一个标准化C++工程模块，辅以规范，以提高工作效率和代码质量。

我们希望不断完善一个出一个最适合c++开发与debug的工程结构框架，以提高工作效率，提高代码质量，通过高质量模块复用来降低项目工期。

# 特性

1. 支持 unit test 例子级别测试
2. 支持 roslaunch 启动后， attach配置

# 插件选择

请在vscode中安装如下插件，

1. cmake & cmake tools
2. Doxygen Documentation Generator
3. C/C++
4. Output Colozier，用于将输出信息颜色化，易于读取
5. C++ TestMate，用于支持测试示例level的调试
4. clang-format，用于c++程序格式化
5. cmake-format，用于cmakelist格式化
6. gitmoji

# 使用注意事项

## moduels编写
1. modules用于存放各功能无关独立子模块
2. 子模块均要求打包为类，以方面进行掉用
3. 各子模块，均需构造 xxx__test.cc 进行完备功能测试，参考示例链接 gtest 库
4. 若子模块需要测试数据，请创建 data 文件夹并在其中读取

## Doxygen编写
1. 请安装Doxygen 注释自动生成插件
2. 所有编写 .h .cc .cpp文件均需要生成 文件注释信息
3. .h 中需生成 类及方法、成员的注释信息，具体请参考示例

## 命名规范
1. 本部分规范完全参照[google style](https://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/naming/) 下面进行说明
2. 文件命名规则： 同google style ， 另补充以下约定： 以下划线进行分割，对于含main函数的源文件，以.cpp文件结尾
3. 类命名规则: 同google style
4. 变量命名：同google style，具体项约定为 小写+下划线。
5. 常量命名： 同google style
5. 函数命名： 同google style
6. 命名空间命名： 项目名+层次+具体模块， eg: namespace templates::modules::submodule1

## 注释规范
1. 统一使用doxygen插件生成
2. .cc 和 .cpp 文件只需进行文件注释
3. .h 需进行文件，类，成员注释

## 结构划分
1. modules用于存放功能子模块，或者消息类。不涉及具体业务流程实现
2. app中存放业务流程相关信息，用于将各modules拼接起来，以实现最终功能

## 依赖规范

1. 引入层与包的概念，层间只允许上游依赖下游，层内可相互依赖

## 变量

1. 变量尽可能少，非必要不增加 类内变量
2. 各个子模块应该包含自身状态的定义  以提供给外界直接判断 而不能外面直接使用 0 1 2 3 4 5 

## git 提交规范

1. 使用 gitmoji 模块选择 特性， 后跟模块类别， 以及说明
2. 为保证主分支不交叉，禁止主分支merge 到分支上，提交时候，首先pull，然后切换到主分支 最后merge into 主分支
3. 请保证每一次提交代码，修改部分均经过format

## 禁止使用launch
1. 用gflags代替 参数配置使用yaml
2. modules 


# TODO

[] 补充app示例
[] 支持ros进入本结构