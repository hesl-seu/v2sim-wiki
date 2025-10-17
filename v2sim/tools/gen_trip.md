# Vehicle and Trip Generator

Command:
```
python gen_trip.py \
    -d <project folder> \
    -n <number of vehicles> \
    [-c <trip parameter folder>] \
    [-v <V2G probability>] \
    [--day <number of trip days>] \
    [--mode <auto|taz|poly>] \
    [--cache-route <none|runtime|static>] \
    [--seed <randomization seed>] \
```

+ **d**: Project folder. e.g. `cases/std_12nodes`
+ **n**: Number of vehicles to generate. e.g. `10000`
+ **c**: Trip parameter folder. Default: `v2sim/probtable`.
+ **v**: Probability of each user willing to participate in V2G. Default: `1.0`.
+ **day**: Number of days for trip generation. Default: `7`. ***Note***: An additional day will be generated. Please read [genveh page](genveh) for details.
+ **mode**: by which to generate the trips. There are 3 modes, which is described in [genveh page](genveh). Default: `auto`.
+ **cache-route**: how to store the routing cache, which is described in [genveh page](genveh). Default: `none`.
+ **seed**: randomization seed. Default: current time in nanosecond.