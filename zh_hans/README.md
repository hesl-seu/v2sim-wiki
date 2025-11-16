# V2Sim

V2Sim 是一个用于城市电力和交通网络耦合仿真的开源车对网仿真平台。它有两个分支，在交通仿真部分有所不同。

+ [**V2Sim**](https://github.com/hesl-seu/h2sim)：使用 SUMO 进行**微观**交通仿真。它可以**识别单个车辆的微观运动**，包括其车道、速度、加速度等。如果您的研究关注电动汽车精细运动对电网的影响，此版本是合适的。代码链接：https://github.com/hesl-seu/h2sim

+ [**V2Sim-UX**](https://github.com/hesl-seu/h2sim/tree/uxsim)：使用 UXsim 进行**中观**交通仿真。它使用自由线程时 Python (3.14+) **运行非常快**。如果您的研究需要快速迭代并关注交通流的整体影响，此版本是合适的。代码链接：https://github.com/hesl-seu/h2sim/tree/uxsim

请按左侧边栏所示的顺序阅读以快速入门。**由于 V2Sim 是一个大型项目，当您首次启动 V2Sim/V2Sim-UX 时，主窗口弹出可能需要长达数分钟的时间。**

## 引用 V2Sim
如果您正在使用 V2Sim 或 V2Sim-UX，请引用我们的文章：V2Sim: An Open-Source Microscopic V2G Simulation Platform in Urban Power and Transportation Network。论文链接：https://ieeexplore.ieee.org/document/10970754

BIB 格式：
```
@ARTICLE{10970754,
  author={Qian, Tao and Fang, Mingyu and Hu, Qinran and Shao, Chengcheng and Zheng, Junyi},
  journal={IEEE Transactions on Smart Grid}, 
  title={V2Sim: An Open-Source Microscopic V2G Simulation Platform in Urban Power and Transportation Network}, 
  year={2025},
  volume={16},
  number={4},
  pages={3167-3178},
  keywords={Vehicle-to-grid;Partial discharges;Microscopy;Batteries;Planning;Discharges (electric);Optimization;Vehicle dynamics;Transportation;Roads;EV charging load simulation;microscopic EV behavior;vehicle-to-grid;charging station fault sensing},
  doi={10.1109/TSG.2025.3560976}}
```

## 术语表

| 术语 | 缩写 | 解释 |
|---|---|---|
| 电动汽车 | EV | 使用电力电池的车辆 |
| 电动汽车充电负荷 | EVCL | 为电动汽车充电的电力负荷 |
| 电动汽车充电站 | EVCS | 为电动汽车电池充电的场所 |
| 快充站 | FCS | 提供相对较高充电功率且支持排队功能的电动汽车充电站 |
| 慢充站 | SCS | 提供相对较低充电功率且不支持排队功能的电动汽车充电站 |
| 荷电状态 | SoC | 电动汽车电池剩余电量的百分比 |
| 充电桩数量 | NoC | 电动汽车充电站（快充站或慢充站）内的充电桩数量 |
| 用户充电价格 | UCP | 向用户收取的单位电量充电费用 |
| 用户放电收益 | UDR | 用户将电动汽车电力回馈至电网所获得的单位电量收益 |
| 停运时间范围 | OTR | 电动汽车充电站不服务任何电动汽车的时间范围 |
| 运行中 | - | 电动汽车充电站正在为电动汽车服务的状态 |
| 用户估计的距离 | UED | 用户估计的从其当前位置到指定地点的路线长度。可能长于或短于实际路线。 |
| 真实行驶距离 | RDD | 从当前位置到指定地点的实际路线长度 |
| 可达 | - | 指某个地点的状态，即用户估计的到该地点的距离短于电动汽车当前荷电状态下的续航里程 |