# Quick Plotting Tool
This tool generates visualization plots from simulation results data. It processes the result files and creates various charts for different power system components.

### Basic Usage
```bash
v2sim-plot -d <input_directory> [options]
```

### Required Arguments
- `-d <directory>`: Specify the input directory containing simulation results

### Main Options
- `-c`: Clear existing plot files before generating new ones
- `-r`: Recursive mode - process all subdirectories
- `-q`: Quick mode - generate only summary plots, skip individual component plots
- `-b <time>`: Begin time for plotting (default: 0)
- `-e <time>`: End time for plotting (default: -1, meaning until end)
- `--plotmax`: Enable maximum plotting (affects FCS/SCS accumulation plots)

### Component-specific Plot Options

**FCS (Fast Charging Station)**
- `--fcs-wcnt`: Plot waiting count
- `--fcs-load`: Plot load
- `--fcs-price`: Plot price

**SCS (Slow Charging Station)**
- `--scs-wcnt`: Plot waiting count
- `--scs-cload`: Plot charging load
- `--scs-dload`: Plot discharging load
- `--scs-netload`: Plot net load
- `--scs-v2gcap`: Plot V2G capacity
- `--scs-pbuy`: Plot buy price
- `--scs-psell`: Plot sell price

**Bus**
- `--bus-activel`: Plot active load
- `--bus-reactivel`: Plot reactive load
- `--bus-volt`: Plot voltage
- `--bus-activeg`: Plot active generation
- `--bus-reactiveg`: Plot reactive generation

**Generator**
- `--gen-active`: Plot active power
- `--gen-reactive`: Plot reactive power
- `--gen-costp`: Plot cost

**Line**
- `--line-active`: Plot active power
- `--line-reactive`: Plot reactive power
- `--line-current`: Plot current

**PVW (Photovoltaic/Wind)**
- `--pvw-P`: Plot power
- `--pvw-cr`: Plot capacity ratio

**ESS (Energy Storage System)**
- `--ess-P`: Plot power
- `--ess-soc`: Plot state of charge

### Examples

1. **Generate all plots in a directory:**
   ```bash
   v2sim-plot -d /path/to/results
   ```

2. **Clear and regenerate plots recursively:**
   ```bash
   v2sim-plot -d /path/to/results -c -r
   ```

3. **Quick mode with specific time range:**
   ```bash
   v2sim-plot -d /path/to/results -q -b 100 -e 1000
   ```

4. **Plot only FCS and bus data:**
   ```bash
   v2sim-plot -d /path/to/results -fcs-wcnt -fcs-load -bus-volt -bus-activel
   ```

### Output
- Creates a `figures` directory in the results folder
- Generates both summary plots and individual component plots
- Provides progress feedback during plotting process
- Handles missing result files gracefully

### Notes
- The tool requires `cproc.clog` files to be present in the input directory
- If no plot options are specified, no plots will be generated
- The tool supports both individual directory processing and recursive directory traversal
- Error messages are provided for invalid arguments or missing input directories