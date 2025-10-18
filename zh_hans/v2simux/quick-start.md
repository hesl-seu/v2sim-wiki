# 快速开始

## A. 环境配置

1.  安装 **Free-threading** Python 3.14：访问 `https://www.python.org/download` 获取 Python。请注意，安装 Python 3.14 时，您必须勾选 `download free-threading binaries` 选项。如果您不想使用并行加速，最低要求版本为 Python 3.9。

2.  **下载本仓库**。您可以代码页面的 `download` 按钮下载仓库。或者，您可以安装 `git` 并使用以下命令克隆此仓库。关于使用 `git` 的官方教程在[这里](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)。
```bash
git clone --single-branch -b uxsim https://github.com/fmy-xfk/v2sim.git
```

3.  **创建 `venv` 虚拟环境**。您可以使用以下命令或 IDE 来完成此操作，但在此步骤中**不要**安装 `requirements.txt` 或 `pyproject.toml` 中的任何包。
```bash
cd v2sim
# 对于 free-threading Python 3.14，运行此命令创建
python3.14t -m venv ./.venv
# 对于普通 Python，运行此命令创建
python -m venv ./.venv
```

4.  **激活虚拟环境**。根据您的情况选择相应的脚本。进入虚拟环境后，如果您想使用 free-threading 功能，请运行 `Python` 并确保其输出包含 `free threading` 相关信息。
```bash
# Windows CMD
./.venv/Scripts/activate.bat
# Windows Powershell
./.venv/Scripts/activate.ps1
# Linux/MacOS
source ./.venv/bin/activate
```

5.  **安装必要的包**。确保您已安装随 Python 附带的 `pip`。如果您不需要并行加速，可以直接使用 `pip install -r requirements.txt` 安装。对于 free-threading Python 3.14，由于它发布不久，一些包（如 PyQt5）尚未支持。请使用以下命令：
```bash
python -m ensurepip
# 对于使用 free-threading Python 3.14 的 Windows 系统
./install_deps.bat
# 对于使用 free-threading Python 3.14 的 Linux/MacOS 系统
mv install_deps.bat install_deps.sh
chmod u+x install_deps.sh
./install_deps.sh
```

## B. 从 V2Sim 创建案例
在 `cases` 文件夹中有 3 个预定义的案例。您可以直接使用这 3 个案例，或者从头开始创建一个新案例。

如果您有一个现有的 v2sim（微观版本）案例，可以使用 `gui_convert.py` 进行转换。车辆、充电站将不会被保留。

V2Sim-UX 的其他部分与 V2Sim 大体相似。请参考 [V2Sim 文档](zh_hans/v2sim/quick-start)。