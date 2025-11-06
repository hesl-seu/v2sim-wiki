# Quick Start

## A. Setup the environment

1. Setup **Free-threading** Python 3.14: Visit `https://www.python.org/download` to get Python. Note that you have to tick the box of `download free-threading binaries` when installing Python 3.14. If you do not want to use parallel acceleration, the minimal version required is Python 3.9.

2. **Download this repo**. You can directly download the code by clicking the `download` button on Github page. Or you can setup `git` and clone the code by the following command. The official tutorial about using `git` is [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
```bash
git clone --single-branch -b uxsim https://github.com/fmy-xfk/v2sim.git
```

3. **Create a `venv` virtual environment**. You can use the following command or IDE to accomplish this, but do **NOT** install any packages from `requirements.txt` or `pyproject.toml` at this step.
```bash
cd v2sim
# For free-threading Python 3.14, run this command to create
python3.14t -m venv ./.venv
# For normal Python, run this command to create
python -m venv ./.venv
```
4. **Activate virtual environment**. Select proper script according to your situation. After entering the virtual environment, run `Python` and ensure it containing messages like `free threading` if you want to use 
```bash
# Windows CMD
./.venv/Scripts/activate.bat
# Windows Powershell
./.venv/Scripts/activate.ps1
# Linux/MacOS
source ./.venv/bin/activate
```

5. **Install necessary packages**. Ensure you have installed `pip` together with Python, and please use following commands:
```bash
python -m ensurepip
pip install -r requirements.txt
```

## B. Create a case from V2Sim
There are 3 pre-defined cases in the `cases` folder. You can exploit the 3 cases directly.

Currently, V2Sim-UX do not support create a case from scratch. But If you have an existing v2sim (microscopic version) case, you can use `gui_convert.py` to convert it. Vehicles, charging stations will not be preserved.

Other parts of V2Sim-UX are generally similar to V2Sim. Please refer to [V2Sim documents](v2sim/quick-start?id=b-create-a-case).