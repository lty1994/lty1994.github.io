---
title: ROS常用命令记录
date: 2019-04-17 16:02:41
tags:
- ROS
- Linux
categories:
- 学习
- ROS
---

## 主机和相关组件：roscore

**roscore**是一个**向节点提供连接信息**，以便节点间可以相互传递信息的**服务程序**。同时，它也提供了一个被节点广泛用于程序配置的**参数服务器**（参数服务器允许节点存储和获取任意数据结构，有命令rosparam用于交互）。它主要作用在于：

- 每个节点都在启动时连接到roscore并**注册**该节点发布和订阅的消息
- roscore向新节点提供与其他**发布并订阅相同消息主题的节点**建立**点对点连接**的必要信息
- **每一个ROS都需要roscore**，否则节点之间无法互相找到

当ROS启动时，我们会首先执行roscore，该进程含有一个名为ROS_MASTER_URI的环境变量，这是一个格式为http://hostname:11311/的字符串，表示roscore有一个实例可以通过这个URI在网络中进行访问。而roscore实际上则启动了三个工具：

1. 主机，处理系统中的各类名称管理。维护一个关于节点名称和网络地址的映射列表。
2. 参数服务器，管理系统中各参数设置的键值对数据
3. rosout节点，收集其他各个节点的调试信息

已知roscore的网络位置后，节点在roscore完成注册（提供节点的网络地址）并请求roscore通过命名找到其他节点和数据流。

- 节点告诉roscore该节点发布和订阅的消息信息
- roscore向节点提供相关消息的生产者和使用者的地址

![roscore与其他节点短暂连接](<https://charlyhuangrostutorial.files.wordpress.com/2016/09/a.png?w=599&h=340>)

上图中的节点周期性的调用roscore来确保相互之间能被找到，而交换点到点消息是两个节点直接完成的。

## 参数管理：rosparam

## 文件系统：

### roscd

### rospack

> rospack find <package> 查找包的路径

## 节点启动

### 单节点启动：rosrun

### 多节点启动：roslaunch

## 多节点系统测试：rostest

## 系统监控

### 节点监控：rosnode

### 话题监控：rostopic

### 服务监控：rosservice

### 服务请求和响应类型：rossrv

## 日志消息

### /rosout和/rosout_agg

## 数据记录和分析

### rosbag

### rostopic echo -b

## 可视化工具

### 日志消息：rqt_console

### 节点、话题和连接：rqt_graph

### 数据图表：rqt_plot

### 话题数据可视化：rqt_bag

### 仿真工具：rviz



