# V2Sim

V2Sim is an open-Source V2G simulation platform for coupled urban power and transportation network. It has following features:

1. Power distribution network simulation, provided by an external package FPowerKit.
2. Transportation simulation, powered by SUMO (microscopic simulation, precise) and UXsim (mesoscopic simulation, fast).
3. EV charging and V2G discharging simulation.

Please read in sequence as shown in the left sidebar to have a quick start. **As V2Sim is a big project, when you start up V2Sim/V2Sim-UX for the first time, it could take up to minutes before the main window pops up.**

Code repository: https://github.com/hesl-seu/v2sim


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

## Nomenclature

|Terminology|Abbreviation|Explanation|
|---|---|---|
|electric vehicle|EV|vehicle with a electric battery|
|EV charging load|EVCL|the power load for EV charging|
|EV charging station|ECVS|a place for EVs to recharge their batteries|
|fast charging station|FCS|an EVCS providing relatively high charging power and supporting queuing|
|slow charging station|SCS|an EVCS providing relatively low charging power and not supporting queuing|
|state of charging|SoC|percentage of how much electricity in an EV battery|
|number of chargers|NoC|Num of chargers in an EVCS (either FCS or SCS)|
|user charging price|UCP|cost of unit electricity charged to an EV for a user|
|user discharging revenue|UDR|revenue of unit electricity discharged to the power grid from an EV for a user|
|offline time range|OTR|Time range when an EVCS does not serve any EV|
|online|-|A status for an EVCS, in which the EVCS serves EVs|
|user-estimated distance|UED|the route length from current position to a given place estimated by the user. May be longer or shorter than the real route.|
|real driving distance|RDD|real route length from current position to a given place|
|reachable|-|A status for a place that the UED to this place is shorter than the range of the EV at current SoC|