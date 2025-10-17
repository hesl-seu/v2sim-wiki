# Use V2Sim as a Library
You can use V2Sim core module as a library! A `pyproject.toml` is supplied for you to make V2Sim as a library.

## For ordinary users
If you'd like to use the raw version of V2Sim, you can package V2Sim as a `.whl` file, and then use `pip` to install it. A minimum version of Python 3.12 is required. 

```bash
# Make a wheel package
python -m build -w

# Install the package. Remeber to replace the <package_name> with actual file name!
pip install <package_name>.whl
```

## For developers
If you'd like to modify V2Sim source code, and meanwhile use it as a package, then you should use the following commands:

```bash
python -m pip install -e .
```

In this way a soft link is create, and you can edit your code without reinstall the package.