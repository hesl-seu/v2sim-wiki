# V2Sim

V2Sim is an open-Source V2G simulation platform for coupled urban power and transportation network. This webpage is a technology documentation of V2Sim.

V2Sim has 2 branches, which are different in the transportation simulation part.

+ **[V2Sim](https://github.com/hesl-seu/v2sim)**: Use SUMO for **microscopic** traffic simulation. It could **identify the microscopic motion of a single vehicle**, including its lane, speed, accerlation, etc. This version is suitable if your research concerns the implication of delicate motion of EVs on power grid. ([Code](https://github.com/hesl-seu/v2sim)|[Wiki](v2sim/))

+ **[V2Sim-UX](https://github.com/hesl-seu/v2sim/tree/uxsim)**: Use UXsim for **mesoscopic** traffic simulation. It **runs very fast** with free-threading Python (3.14+). This version is suitable if your research need fast iterations and focus on the overall implication of the traffic flow. ([Code](https://github.com/hesl-seu/v2sim/tree/uxsim)|[Wiki](v2simux/))

## Cite V2Sim
If you are using V2Sim or V2Sim-UX, please cite our article: V2Sim: An Open-Source Microscopic V2G Simulation Platform in Urban Power and Transportation Network. Paper link: https://ieeexplore.ieee.org/document/10970754

BIB format:
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