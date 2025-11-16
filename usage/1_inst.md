# Installation
V2Sim and V2Sim-UX are both Python programs. Visit `https://www.python.org/download` to get Python.
+ Note 1: V2Sim-UX has 2 running modes, the normal mode and the parallel mode, which requires different environments. 
+ Note 2: V2Sim and V2Sim-UX requires different version of Python, which is listed below:

<!-- tabs:start -->

# **V2Sim**
+ Minimum version required: 3.9
+ Maximum version required: 3.13

Note that the newest Python 3.14 is not supported due to `libsumo` compatibility.

# **V2Sim-UX**
+ Minimum version required: 3.9

# **V2Sim-UX (Parallel)**
+ Minimum version required: 3.14t (Free-threading version)
+ Please tick "Download free-threading binaries" when installing Python.

<!-- tabs:end -->

There two ways to install V2Sim and V2Sim-UX: (1) From PyPI and (2) From source. You can select the way you preferred.

<!-- tabs:start -->
### **From PyPI**

Both V2Sim and V2Sim-UX have been uploaded to PyPI. You can directly install them by the following command:

<!-- tabs:start -->
# **V2Sim**

```bash
pip install v2sim
```

If you want to use the power distribution network (PDN) simulation, please install `cvxpy` and `ecos` in addition:

```bash
pip install cvxpy ecos
```

# **V2Sim-UX**

```bash
pip install --extra-index-url https://pypi.anaconda.org/scientific-python-nightly-wheels/simple v2simux 
```

# **V2Sim-UX (Parallel)**
As the packages used by free-threading version is different from the GIL-enabled version, please create a virtual environment to avoid spoiling your Python.

```bash
mkdir ./v2sim
cd ./v2sim
python3.14t -m venv ./.venv

# Windows CMD
./.venv/Scripts/activate.bat
# Windows Powershell
./.venv/Scripts/activate.ps1
# Linux/MacOS
source ./.venv/bin/activate

pip install --extra-index-url https://pypi.anaconda.org/scientific-python-nightly-wheels/simple v2simux 
```
<!-- tabs:end -->

### **From source**
1. **Download code**. You can directly download code by clicking the `download` button on GitHub page, or you can setup `git` and clone the repository by one of the following command. The official tutorial about using `git` is [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

<!-- tabs:start -->
# **V2Sim**

```bash
git clone -b main --single-branch https://github.com/fmy-xfk/v2sim.git
```

# **V2Sim-UX**

```bash
git clone -b uxsim --single-branch https://github.com/fmy-xfk/v2sim.git
```

# **V2Sim-UX (Parallel)**

```bash
git clone -b uxsim --single-branch https://github.com/fmy-xfk/v2sim.git
```
<!-- tabs:end -->


2. **Setup necessary packages**. Ensure you have installed `pip` together with Python. Then install dependencies and V2Sim/V2Sim-UX by the following commands:

<!-- tabs:start -->
# **V2Sim**

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
# **V2Sim-UX**

```bash
cd ./v2sim
python -m ensurepip
pip install -r requirements.txt
pip install -e .
```
# **V2Sim-UX (Parallel)**
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
If you have installed from the source, you will find 3 pre-defined cases in the `cases` folder: `std_12nodes`, `std_37nodes`, and `std_Nanjing`. V2Sim will have an extra `pwe_37nodes`. You can utilize these cases directly, or create a new case from scratch by following sections.