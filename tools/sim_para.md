# Multi-process parallel simulation tool

<details>
<summary>This tool is not supplied in current version. The hidden information only works for the old version.</summary>
Command:

```
v2sim-para \
    [-p <maximum number of parallel tasks>] \
    [-r <output folder>] \
    [-f <command line file>] \
    [-c <simulation parameter command line>] \
    [-n <number of simulations>]
```

+ p: maximum number of parallel tasks, default is "CPU core number - 1"
+ r: output folder, default is results_set
+ The following two groups of parameters are mutually exclusive:
+ Group 1:
  - f: read simulation parameters from a file, each line of this file is a simulation parameter command line, the format of the simulation parameter command line is as follows
+ Group 2:
  - c: simulation parameter command line, the format is the same as the parameter of main.py, but all '-seed' parameters must be omitted (the '-seed' parameter is automatically assigned by the parallel simulator)
  - n: number of simulations, default is 50
    
+ Example parameters:
  - `-p 7 -d 37nodes -r results_set -c "-gen-veh '-n 5000' -gen-fcs '-pbuy 1.5 -slots 10' -gen-scs '-pbuy 1.5 -slots 10'" -n 50`
</details>