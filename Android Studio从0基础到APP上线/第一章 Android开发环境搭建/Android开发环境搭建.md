## 1.2 搭建Android Studio开发环境

### 1.2.1 计算机配置要求

- 硬件要求
- 网络要求

### 1.2.2 安装Android Studio

- 下载小海豚版本Android Studio
- 下载后进行安装

### 1.2.3 下载Android的SDK

SDK是用于编译app的开发包

- Tools- SDK manager可以下载SDK
- Android SDK目录说明
  - build-tools目录：存放各个版本的编译工具
  - emulator目录：存放模拟器的管理工具
  - platforms：存放各个Android版本的资源文件
  - platform-tools：常用开发辅助工具，包括客户端驱动程序adb.exe、数据库管理工具sqlite3.exe
  - sources：存放各个版本的SDK源码

## 1.3 创建并编译App工程

### 1.3.1 创建新项目

略

### 1.3.2 导入已有工程

可以以app形式导入：直接file-open

也可以以moudle形式导入：file-new-import moudle

### 1.3.3 编译App工程

Build-make project，会编译整个项目的所有模块

build-make module xxx，会编译指定名字的模块

build-clean project，在build-rebuild project，表示先清理项目，再进行编译

## 1.4 运行和调试App

### 1.4.1 创建内置模拟器

tools-device manager

### 1.4.2 在模拟器上运行App

安装完成后直接运行app

### 1.4.3 观察App的运行日志

分成5个级别，分别是e、w、d、i、v

一般使用d即可

