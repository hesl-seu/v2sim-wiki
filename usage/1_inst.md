# Installation
V2Sim is a Python program. Visit `https://www.python.org/download` to get Python. The minimum version required is Python 3.9.

+ **Note:** When you use UXsim as the transportation simulator in the parallel mode, the minimum version required is Python 3.14t. Here `t` means the free-threading version. Please tick "Download free-threading binaries" when installing Python.

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
Use the following commands to start simulation:

```bash
# Start up GUI
v2sim-gui

# Start simulation from command prompt
v2sim -d <case folder>
```

Note that the transporation simulator employed and the parallel mode are determined by your case configuration.