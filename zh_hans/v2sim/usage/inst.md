# 安装指南

## 克隆代码仓库
使用 GIT 克隆此代码仓库：

```bash
git clone https://gitee.com/fmy-xfk/v2sim.git
```

## 安装软件

本平台需要以下软件：

| 软件名称 | 版本要求 | 官方网站 |
|---|---|---|
| Python | >=3.8 | https://www.python.org/download |
| SUMO | >=1.18 | https://eclipse.dev/sumo |

## 安装 Python 包
所需的 Python 包已记录在 `requirements.txt` 文件中。

**图形界面方法：** 您可以直接启动 `gui_main.py`，程序会自动检测所需的包是否已正确安装。如果缺少必要的包，会弹出一个提示对话框显示缺失的包：

![bad_req](https://github.com/user-attachments/assets/672f18e8-946c-4995-a305-5b20f5711fde)

点击 `安装并重试` 来安装这些包，然后程序会自动重启。

**命令行方法：** 使用以下命令可以方便地安装所有必需的包：

```bash
pip install -r requirements.txt
```

如果您需要使用配电网仿真功能，请额外安装 `cvxpy` 和 `ecos`：

```bash
pip install cvxpy ecos
```