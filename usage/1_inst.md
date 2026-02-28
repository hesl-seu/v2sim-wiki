# Installation

## Prepare Python
V2Sim is a **Python** program. You can visit `https://www.python.org/download` to get Python. Before you download Python, we would like to inform you of the difference of the transportation simulator used in V2Sim.

+ **SUMO**: A simulator for microscopic simulation. It could identify the microscopic motion of a single vehicle, including its lane, speed, accerlation, etc. This version is suitable if your research concerns the implication of delicate motion of EVs on power grid.
+ **UXsim**: A simulator for mesocopic simulation. It runs much faster compared to SUMO, but the details of vehicles are less precise. This version is suitable if your research need fast iterations and focus on the overall implication of the traffic flow.

V2Sim treats the road network as a whole by **default**. Besides, V2Sim has a **parallel** mode by splitting road network into regions to boost simulation. This function requires free-threading Python. Here is a table depicting the version required for Python.

|Feature|SUMO|UXsim|
|---|---|---|
|Default|>=3.9|>=3.9|
|Parallel|Not supported|>=3.14t|

## Get V2Sim
There two ways to install V2Sim: (1) From PyPI and (2) From source. You can select the way you preferred.

<!-- tabs:start -->
### **From PyPI**

Both V2Sim and V2Sim-UX have been uploaded to PyPI. You can directly install them by the following command:

```bash
pip install v2sim
```

If you want to use the power distribution network (PDN) simulation, please install `cvxpy` (an convex-optimizer) and `cbc` (a solver) in addition:

```bash
pip install cvxpy cbc
```

### **From source**
1. **Download code**. You can directly download code by clicking the `download` button on GitHub page, or you can setup `git` and clone the repository by one of the following command. The official tutorial about using `git` is [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

```bash
git clone -b main --single-branch https://github.com/fmy-xfk/v2sim.git
```

2. **Setup necessary packages**. Ensure you have installed `pip` together with Python. Then install dependencies and V2Sim/V2Sim-UX by the following commands:

<!-- tabs:start -->
# **Normal Python**

```bash
cd ./v2sim
python -m ensurepip
pip install -r requirements.txt
pip install -e .
```

If you want to use the power distribution network (PDN) simulation, please install `cvxpy` and `ecos` in addition:

```bash
pip install cvxpy ecos
```
# **Free-threading Python**

Firstly, create and activate a `venv` virtual environment.
```bash
cd ./v2sim
python3.14t -m venv ./.venv

# Windows CMD
./.venv/Scripts/activate.bat
# Windows Powershell
./.venv/Scripts/activate.ps1
# Linux/MacOS
source ./.venv/bin/activate
```

Then, ensure your `pip` is installed, and then install the dependencies.
```bash
python -m ensurepip
pip install -r requirements.txt
pip install -e .
```
<!-- tabs:end -->

<!-- tabs:end -->

## Pre-defined cases
If you have installed from the source, you will find 6 pre-defined cases in the `cases` folder: `sumo_12nodes`, `sumo_37nodes`, and `sumo_Nanjing` for SUMO, and `ux_12nodes`, `ux_37nodes`, and `ux_Nanjing` for UXsim. You can utilize these cases directly, or create a new case from scratch by following sections.

## Run program
V2Sim provides both GUI and command line tools.

To start up GUI, use the following command:
```
v2sim-gui
```
You will see a window like this:

![welcome](../imgs/0.png)

To directly start simulation in the command line:
```
v2sim -d <case folder>
```