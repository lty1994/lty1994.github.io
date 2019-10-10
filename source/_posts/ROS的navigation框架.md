---
title: ROS的navigation框架
date: 2019-05-11 13:47:42
tags:
- 导航
- Linux
categories:
- 学习
- ROS
---

### 前言

当我们有了环境地图之后，我们就能够使用ros的导航框架进行导航。导航的框架如下图所示：

![导航框架](https://onwzuw.ch.files.1drv.com/y4m-IaKjwGlJ-PJ1yKmVqZf1M5fP0DMjAqjyg9WxFU8x40ovxvcb5CBdoaJkAnlR9X-Ci0B6nuWFJTZUzHX8HH6HRKdWrisTg2DWFbe1RXu5DExXZE_L_DS_HnG9RQlza96CLHRdHtNyZK78FsI6m1lHFpu5dOlPYVR_MWpu1yHcwF1uvIUTQVtmVX5FZ2AZG-GJnEVl8gaKgyZ0fqpZ69JUA?width=999&height=435&cropmode=none)

### 框架介绍

框架由若干组件组成，其中白底的部分是由导航框架提供的组件，蓝底部分则是由平台提供的组件，灰色的组件则是可选的组件。从框架结构可看出核心的部分在于move_base，其余组件则是辅助move_base进行导航。下面简单介绍一下框架的核心包move_base：

#### Move_base

它的外部输入消息主要由几个部分组成：

- OD点信息，其中目标在单次导航中固定不变，实时位置则是时刻变化的；
- 地图服务，它主要作用是用于生成全局的价值地图；
- 传感信息，它的主要作用是用于生成局部的价值地图；同时也能够辅助生成全局的价值图；
- tf消息，作用是提供各个坐标系的转换关系
- 里程计信息，一个作用是辅助生成当前位置的坐标，另外就是用于辅助局部路径规划
- 控制信息，导航系统最终输出的实时控制信息

它的内部组件主要由以下部分组成：

- local_costmap，根据传感器信息实时更新的局部价值图，local_planner根据这个价值图作局部的路径规划，实现动态避障的效果；
- global_costmap，根据地图服务提供的地图信息生成的全局的价值图，global_planner根据这个价值图和输入的OD信息生成实时的全局的路径；
- local_planner，局部的路径规划，有几种实现的算法，最常用的是动态窗口法；它将驱动平台沿着global_planner生成的路径行走，同时使用传感器的信息，进行避障。如果无法规划出局部的路径，那么它将请求global_planner重新规划路线，并尝试重新跟随新的路径；
- global_planner，全局的路径规划，常见实现的算法就是A*和Dijkstra算法；

在对move_base进行了介绍之后，我们将对其中的几个关键节点进行详细的解释，同时对于坐标系转换以及定位也作一个详细的解释。因为它们对于单车导航来说至关重要，我们研究的局部规划也是基于此框架来进行算法的设计和开发。对于每个功能，我们介绍它的输入、输出，消息结构的定义以及实现的算法。

### 框架内功能包

#### 局部路径规划

局部路径规划是我们主要聚焦的内容，因为在单次OD导航的过程中，对于环境动态变化的响应是由局部路径规划生成的。它提供的功能主要有两个：

1. 沿着全局路径规划的路径进行行走
2. 借助传感器感知的环境信息实现对路径上的动态障碍进行避让
3. 向

### 

