# V2Sim

V2Sim是开源的考虑车网互动(V2G)的城市交通网-电力网仿真软件。本网站是V2Sim的文档页面。

V2Sim现有2个分支，两者在交通网仿真部分不同。

+ **[V2Sim](https://github.com/hesl-seu/v2sim)**: 该版本使用SUMO进行**微观**交通网仿真，计算**每一辆车的微观动作**，包括车道、车速、加速度等参数。这个分支适用于需要考虑精细电动汽车动作对电网影响的研究。([代码](https://github.com/hesl-seu/v2sim)|[文档](zh_hans/v2sim/))

+ **[V2Sim-UX](https://github.com/hesl-seu/v2sim/tree/uxsim)**: 该版本使用UXsim进行**中观**交通流仿真，在自由线程版Python(3.14+)上**运行速度很快**。这个分支适用于关注交通流整体影响或者需要快速迭代的研究。([代码](https://github.com/hesl-seu/v2sim/tree/uxsim)|[文档](zh_hans/v2simux/))


## 引用V2Sim
如果您正在使用V2Sim或者V2Sim-UX，请引用我们的文章：V2Sim: An Open-Source Microscopic V2G Simulation Platform in Urban Power and Transportation Network
论文链接：https://ieeexplore.ieee.org/document/10970754

国标GB/T7714-2015引用格式：

>Qian Tao, Fang Mingyu, Hu Qinran, et al. V2Sim: An Open-Source Microscopic V2G Simulation Platform in Urban Power and Transportation Network[J]. IEEE Transactions on Smart Grid, 2025,16(4):3167-3178.

BIB格式：
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