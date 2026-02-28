# 安装指南

## 准备 Python
V2Sim 是一个 **Python** 程序。您可以访问 `https://www.python.org/download` 获取 Python。在您下载 Python 之前，我们想告知您 V2Sim 中使用的交通仿真器的区别。

+ **SUMO**：一种用于微观仿真的仿真器。它可以识别单个车辆的微观运动，包括其车道、速度、加速度等。如果您的研究关注电动汽车的精细运动对电网的影响，此版本较为适合。
+ **UXsim**：一种用于中观仿真的仿真器。与 SUMO 相比，它的运行速度快得多，但车辆的细节信息精确度较低。如果您的研究需要快速迭代并关注交通流的整体影响，此版本较为适合。

V2Sim**默认**将路网视为一个整体进行处理。此外，V2Sim 还具有**并行**模式，通过将路网分割成多个区域来加速仿真。此功能需要自由线程 Python。下表描述了 Python 所需的版本。

|功能特性|SUMO|UXsim|
|---|---|---|
|默认|>=3.9|>=3.9|
|并行|不支持|>=3.14t|

## 获取V2Sim

有两种安装 V2Sim 的方法：(1) 从 PyPI 安装和 (2) 从源码安装。您可以选择您喜欢的方式。

<!-- tabs:start -->
### **从 PyPI 安装**

V2Sim 和 V2Sim-UX 都已上传到 PyPI。您可以直接通过以下命令安装：

```bash
pip install v2sim
```

如果您想使用配电网仿真，请额外安装 `cvxpy`（一个凸优化器）和 `cbc`（一个求解器）：

```bash
pip install cvxpy cbc
```

### **从源码安装**
1. **下载代码**。您可以直接点击 GitHub 页面上的 `download` 按钮下载代码，或者安装 `git` 后通过以下命令之一克隆仓库。关于使用 `git` 的官方教程在[这里](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)。

```bash
git clone -b main --single-branch https://github.com/fmy-xfk/v2sim.git
```

2. **安装必要的包**。确保您已安装与 Python 一起的 `pip`。然后通过以下命令安装依赖项和 V2Sim/V2Sim-UX：

<!-- tabs:start -->
# **普通 Python**

```bash
cd ./v2sim
python -m ensurepip
pip install -r requirements.txt
pip install -e .
```

如果您想使用配电网仿真，请额外安装 `cvxpy` 和 `ecos`：

```bash
pip install cvxpy ecos
```
# **自由线程 Python**

首先，创建并激活一个 `venv` 虚拟环境。
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

然后，确保您的 `pip` 已安装，然后安装依赖项。
```bash
python -m ensurepip
pip install -r requirements.txt
pip install -e .
```
<!-- tabs:end -->

<!-- tabs:end -->

## 预定义案例
如果您从源码安装，您将在 `cases` 文件夹中找到 6 个预定义案例：用于 SUMO 的 `sumo_12nodes`、`sumo_37nodes` 和 `sumo_Nanjing`，以及用于 UXsim 的 `ux_12nodes`、`ux_37nodes` 和 `ux_Nanjing`。您可以直接使用这些案例，或者按照后续章节从头创建一个新案例。

## 运行程序
V2Sim 提供图形用户界面和命令行工具两种方式。

要启动图形用户界面，请使用以下命令：
```
v2sim-gui
```
您将看到一个如下所示的窗口：

![欢迎界面](../imgs/0.png)

要在命令行中直接启动仿真，请使用：
```
v2sim -d <案例文件夹>
```