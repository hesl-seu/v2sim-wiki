# Charging station generation program
Command:

```
v2sim-gen-cs \ 
    -d <project folder> \
    [--type <scs|fcs>] \
    [--slots <number of charging piles>] \
    [--pbuy <purchase price>] \
    [--randomize-pbuy] \
    [--psell <selling price>] \
    [--randomize-psell] \
    [--seed <randomization seed>] \
    [--n-cs <number of charging stations>] \
    [--cs-names <CS names>] \
    [--n-bus <number of buses>] \
    [--bus-names <buses used>] \
```

+ d: Project folder
+ type: charging station type, default fcs, optional fcs(fast charging station), scs(slow charging station)
+ slots: number of charging piles, default 10
+ pbuy: purchase price, default 1.5 yuan/kWh
+ randomize-pbuy: randomize purchase price
+ psell: selling price, default 1 yuan/kWh
+ randomize-psell: randomize selling price
+ seed: randomization seed. Default: current time in nanosecond.
+ Charging station selection:
  - By default, all available charging stations are used, which can be changed by the following options:
  - n-cs: number of fast charging stations, specifying this option will make the selection of fast charging stations random
  - cs-names: fast charging station names, specifying this option will make the selection of fast charging stations specified
  - The above two options cannot be used together
+ Bus selection:
  - By default, all buses are used, which can be changed by the following options:
  - n-bus: number of buses used, specifying this option will make the bus selection random
  - bus-names: buses used, specifying this option will make the bus selection specified
  - The above two options cannot be used together