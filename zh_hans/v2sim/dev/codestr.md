# 代码结构

在本代码库中，V2Sim 的核心代码存储在 `v2sim` 文件夹中。此外，`sim_single.py` 提供了启动仿真实例的方法。本文将介绍 `v2sim` 文件夹的结构，该文件夹可以按照上一篇文章的介绍部署为软件包。

+ V2Sim 包
  + traffic: 核心模块
    + cs.py: 充电站，包含 FCS 和 SCS，继承自基类 CS
    + cslist.py: CS 集合的包装器，支持集合操作
    + ev.py: 电动汽车和行程
    + evdict.py: EV 集合的包装器，支持集合操作
    + trip.py: 行程记录器、行程事件监听器和行程日志文件读取器
    + utils.py: 辅助函数和杂项功能
    + inst.py: 整个交通仿真（包含电动汽车和充电站）的包装器
  + trafficgen: 数据生成模块
  + statistics: 数据记录模块
  + plugins: 插件和外部代码调用模块
  + plotkit: 结果绘图模块
  + locale: 语言辅助模块
  + sim_core.py: V2Sim 仿真实例包装器