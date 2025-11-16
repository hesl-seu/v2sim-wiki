# 安装指南

V2Sim 和 V2Sim-UX 都是 Python 程序。请访问 `https://www.python.org/download` 获取 Python。
+ 注意 1：V2Sim-UX 有 2 种运行模式，普通模式和并行模式，需要不同的环境。
+ 注意 2：V2Sim 和 V2Sim-UX 需要不同版本的 Python，如下所列：

<!-- tabs:start -->

# **V2Sim**
+ 最低要求版本：3.9
+ 最高支持版本：3.13

请注意，由于 `libsumo` 兼容性问题，最新的 Python 3.14 不受支持。

# **V2Sim-UX**
+ 最低要求版本：3.9

# **V2Sim-UX (并行模式)**
+ 最低要求版本：3.14t（自由线程版本）
+ 安装 Python 时请勾选 "Download free-threading binaries"。

<!-- tabs:end -->

有两种安装 V2Sim 和 V2Sim-UX 的方法：(1) 从 PyPI 安装和 (2) 从源码安装。您可以选择您喜欢的方式。

<!-- tabs:start -->
### **从 PyPI 安装**

V2Sim 和 V2Sim-UX 都已上传到 PyPI。您可以直接通过以下命令安装：

<!-- tabs:start -->
# **V2Sim**

```bash
pip install v2sim
```

如果您想使用配电网仿真，请额外安装 `cvxpy` 和 `ecos`：

```bash
pip install cvxpy ecos
```

# **V2Sim-UX**

```bash
pip install --extra-index-url https://pypi.anaconda.org/scientific-python-nightly-wheels/simple v2simux 
```

# **V2Sim-UX (并行模式)**
由于自由线程版本使用的包与启用 GIL 的版本不同，请创建一个虚拟环境以避免破坏您的 Python 环境。

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

### **从源码安装**
1. **下载代码**。您可以直接点击 GitHub 页面上的 `download` 按钮下载代码，或者安装 `git` 后通过以下命令之一克隆仓库。关于使用 `git` 的官方教程在[这里](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)。

<!-- tabs:start -->
# **V2Sim**

```bash
git clone -b main --single-branch https://github.com/fmy-xfk/v2sim.git
```

# **V2Sim-UX**

```bash
git clone -b uxsim --single-branch https://github.com/fmy-xfk/v2sim.git
```

# **V2Sim-UX (并行模式)**

```bash
git clone -b uxsim --single-branch https://github.com/fmy-xfk/v2sim.git
```
<!-- tabs:end -->

2. **安装必要的包**。确保您已安装与 Python 一起的 `pip`。然后通过以下命令安装依赖项和 V2Sim/V2Sim-UX：

<!-- tabs:start -->
# **V2Sim**

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
# **V2Sim-UX**

```bash
cd ./v2sim
python -m ensurepip
pip install -r requirements.txt
pip install -e .
```
# **V2Sim-UX (并行模式)**
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
如果您从源码安装，您将在 `cases` 文件夹中找到 3 个预定义案例：`std_12nodes`、`std_37nodes` 和 `std_Nanjing`。V2Sim 还会有一个额外的 `pwe_37nodes`。您可以直接使用这些案例，或者按照后续章节从头创建一个新案例。