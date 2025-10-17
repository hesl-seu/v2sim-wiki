# EV charging load generation simulator - usage help

Command:

```
python sim_single.py \
    -d <configuration folder> \
    [-b <start time>] \
    [-e <end time>] \
    [-l <data recording step>] \
    [-o <output folder>] \
    [--seed <random seed>] \
    [--log <data to be recorded>] \
    [--no-plg <plugins to be disabled>] \
    [--show] \
    [--no-daemon] \
    [--copy] \
    [--copy-state] \
    [--debug] \

python sim_single.py \
    [--file <file>]

python sim_single.py \
    [--ls-com] 
```

+ The following parameters should be used alone and cannot be combined with other parameters:
  - file: Read parameters from an external file and execute command line simulation serially, the content of this file should be a space-separated parameter list, if the file has multiple lines, only the first line will be considered. For example: "-d 12nodes -b 0 -e 172800"
  - ls-com: List all available plugins and recording items.
+ The following parameters are used for single simulation:
  - d: Configuration folder.
  - b: Start time, in seconds. Read from SUMO configuration by default.
  - e: End time, in seconds. Read from SUMO configuration by default.
  - l: Data recording step, in seconds. 10 by default.
  - o: Output folder. The 'results' folder in the current directory is the default output folder.
  - seed: Random seed for generating vehicles, charging stations and SUMO. Current time (ns)%65536 is the default value.
  - log: Data to be recorded, including electric vehicles(ev), fast charging stations(fcs), slow charging stations(scs), generators(gen), buses(bus) and transmission lines(line), multiple data separated by commas, default is "fcs,scs"
  - no-plg: Disable specific plugins. Multiple plugins should separated by commas. No plugins are disabled by default.
  - copy: Copy the configuration file to the output folder after the simulation ends. Do not copy by default.
  - gen-veh: Command line to regenerate the EVs file. If not given, the file won't be regenerated, and this is the default behavior.
  - gen-fcs: Command line to regenerate the FCS file. If not given, the file won't be regenerated, and this is the default behavior.
  - gen-scs: Command line to regenerate the SCS file. If not given, the file won't be regenerated, and this is the default behavior.
  - plot: Command line for figure plotting after simulation.
  - initial-state: Folder of initial state files. If not given, the initial state will be desgined by the default behavior of sumo.
  - load-last-state: Load the saved state of last simulation of this case. If this option is enabled, the initial-state option will be ignored.
  - save-on-abort: Whether to save the state when the simulation is aborted.
  - save-on-finish: Whether to save the state when the simulation is finished.
  - copy-state: Copy the state file to the case folder after the simulation ends. Do not copy by default.
  - route-algo: Route algorithm, default is "CH". Other options are "astar", "dijkstra", "CH", and "CHWrapper".
+ The following parameters are used for graphical simulation:
  - show: Enable this option to start in GUI mode. This option is only useful in Linux. In Windows, please adjust WINDOWS_VISUALIZE in v2sim/traffic/win_vis.py to change the visibility level
  - no-daemon: Enable this option to separate the simulation thread from the display window. When not enabled, the simulation will stop once the display window is closed
  - debug: Enable debug mode for graphical simulation, detailed error information will be output when graphical simulation fails