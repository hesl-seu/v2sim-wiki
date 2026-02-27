# Simulation Tool

## Command Usage

### Main Command
```
v2sim (-d | --dir | --proj-dir <configuration folder>) \
    [-o | --out | --out-dir <output folder>] \
    [-b <start time>] \
    [-l <traffic simulation step>] \
    [-e <end time>] \
    [--break-at <break at time>]
    [--seed <random seed>] \
    [--log | --logging-items <data to be recorded>] \
    [--no-plg | --disable-plugins <plugins to be disabled>] \
    [--gen-veh <regeneration command>] \
    [--gen-fcs <regeneration command>] \
    [--gen-scs <regeneration command>] \
    [--plot-script <plotting command>] \
    [--initial-state <state folder> | --load-last-state | --load-case-state] \
    [--save-on-abort] \
    [--save-on-finish] \
    [--uxsim-show-info] \ 
    [--uxsim-randomize] \ 
    [--uxsim-no-parallel] \ 
    [--sumo-ignore-driving] \ 
    [--sumo-raise-routing-error] \
    [--copy-proj-to-out] \
    [--copy-state-to-proj] \
    [--route-algo <algorithm>] \
    [--show] \
    [--no-daemon] \
    [--debug]
```

### Alternative Python Usage
```bash
v2sim [--file <file>]
v2sim [--ls-com]
```

## Standalone Parameters
The following parameters should be used alone and cannot be combined with other parameters:

- **`--file <file>`**: Read parameters from an external file and execute command line simulation serially. The file content should be a space-separated parameter list. If the file has multiple lines, only the first line will be considered.
  - Example file content: `"-d 12nodes -b 0 -e 172800"`

- **`--ls-com`**: List all available plugins and recording items.

## Single Simulation Parameters

### Core Parameters
- **`-d <configuration folder>`**: **Required**. Configuration folder containing network, vehicle, and charging station files.

- **`-b <start time>`**: Start time in seconds. Read from SUMO configuration by default.

- **`-e <end time>`**: End time in seconds. Read from SUMO configuration by default.

- **`-l <data recording step>`**: Data recording step in seconds. Default is 10 seconds.

- **`-o <output folder>`**: Output folder. The 'results' folder in the current directory is the default output folder.

### Randomization & Logging
- **`--seed <random seed>`**: Random seed for generating vehicles, charging stations and SUMO. Current time (ns)%65536 is the default value.

- **`--log <data to be recorded>`**: Data to be recorded, including:
  - `ev`: Electric vehicles
  - `fcs`: Fast charging stations  
  - `scs`: Slow charging stations
  - `gen`: Generators
  - `bus`: Buses
  - `line`: Transmission lines
  
  Multiple data types separated by commas. Default is "fcs,scs".

### Plugin Management
- **`--no-plg <plugins to be disabled>`**: Disable specific plugins. Multiple plugins should be separated by commas. No plugins are disabled by default.

### File Generation & Regeneration
- **`--gen-veh <command>`**: Command line to regenerate the EVs file. If not given, the file won't be regenerated (default behavior).

- **`--gen-fcs <command>`**: Command line to regenerate the FCS file. If not given, the file won't be regenerated (default behavior).

- **`--gen-scs <command>`**: Command line to regenerate the SCS file. If not given, the file won't be regenerated (default behavior).

### State Management
- **`--initial-state <folder>`**: Folder containing initial state files. If not given, the initial state will be designed by the default behavior of SUMO.

- **`--load-last-state`**: Load the saved state from the last simulation of this case. If this option is enabled, the `initial-state` option will be ignored.

- **`--save-on-abort`**: Save the simulation state when the simulation is aborted (e.g., via Ctrl+C).

- **`--save-on-finish`**: Save the simulation state when the simulation finishes normally.

### Output & Configuration
- **`--copy`**: Copy the configuration files to the output folder after the simulation ends. Not enabled by default.

- **`--copy-state`**: Copy the state files to the case folder after the simulation ends. Not enabled by default.

- **`--plot <command>`**: Command line for figure plotting after simulation completion.

### Routing & Algorithm
- **`--route-algo <algorithm>`**: Routing algorithm. Options are:
  - `"astar"` (default)
  - `"dijkstra"`
  - `"CH"` (Contraction Hierarchies) (V2Sim only)
  - `"CHWrapper"` (V2Sim only)

## Graphical Simulation Parameters (V2Sim only)

- **`--show`**: Enable GUI mode. **Note**: This option is only useful in Linux. In Windows, please adjust `WINDOWS_VISUALIZE` in `v2sim/traffic/win_vis.py` to change the visibility level.

- **`--no-daemon`**: Separate the simulation thread from the display window. When not enabled, the simulation will stop once the display window is closed.

- **`--debug`**: Enable debug mode for graphical simulation. Detailed error information will be output when graphical simulation fails.

## Error Handling

- Invalid parameters will trigger immediate termination with descriptive error messages
- Missing required files will be reported with specific file names
- Simulation interruptions (Ctrl+C) can be configured to save state automatically
- Plugin loading failures are reported but don't necessarily stop simulation