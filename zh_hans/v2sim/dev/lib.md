# 将 V2Sim 作为库使用

您可以将 V2Sim 核心模块作为库使用！我们提供了 `pyproject.toml` 文件，方便您将 V2Sim 制作成库。

## 对于普通用户

如果您想使用原始版本的 V2Sim，可以将 V2Sim 打包为 `.whl` 文件，然后使用 `pip` 进行安装。需要 Python 3.12 或更高版本。

```bash
# 制作 wheel 包
python -m build -w

# 安装包。记得将 <package_name> 替换为实际的文件名！
pip install <package_name>.whl
```

## 对于开发者
如果您想修改 V2Sim 源代码，同时将其作为包使用，应该使用以下命令：

```bash
python -m pip install -e .
```

这种方式会创建一个软链接，您可以在不重新安装包的情况下编辑代码。