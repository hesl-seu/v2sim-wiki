# Installation

## Clone this repository
Clone this repository by GIT:

```bash
git clone https://gitee.com/fmy-xfk/v2sim.git
```

## Install software

The following software are needed by this platform:

|Name|Version|Official Site|
|---|---|---|
|Python|>=3.8|https://www.python.org/download|
|SUMO|>=1.18|https://eclipse.dev/sumo|

## Install python packages
Python packages needed have been recorded in `requirements.txt`.

**GUI method:** You can just start `gui_main.py`, and the program will automatically detect whether the packages are properly installed. If required packages are missing, a notice dialog will pop up and display the missing packages:

![bad_req](https://github.com/user-attachments/assets/672f18e8-946c-4995-a305-5b20f5711fde)

Click `Install and Retry` to setup these packages, and then the program will automatically restart.

**CMD method:** Just use the following command to install them conveniently:

```bash
pip install -r requirements.txt
```

If you want to use the power distribution network (PDN) simulation, please install `cvxpy` and `ecos` in addition:

```bash
pip install cvxpy ecos
```