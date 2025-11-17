**插件与统计项**

您可以为 V2Sim 添加插件和统计项！

插件可以被不同的案例使用。它可用于自定义 V2Sim 的行为，或为 V2Sim 添加新功能。

将插件文件放置在 `/external_components/` 目录下。文件名必须与插件名相同。例如，如果您的插件名为 `demo`，那么文件名必须是 `demo.py`。**请注意，您的插件名称不得与任何已安装的其他软件包同名；否则将无法正确导入。**

插件必须继承 `v2sim/plugins/base.py` 中定义的 `PluginBase` 类。必须包含 `Init` 和 `Work` 方法。前者用于初始化插件，后者用于运行插件。还必须包含 `Description` 属性。以下是一个插件类的示例：

## 导入必要的包
要构建插件和统计项，请导入以下包：
```py
import xml.etree.ElementTree as ET
from pathlib import Path
from typing import Iterable, Any, Tuple, List, Dict
from feasytools import LangLib # 用于多语言支持
from v2sim import TrafficInst
from v2sim.plugins import *
from v2sim.statistics import *
# 如果您使用的是 V2Sim-UX，请将 v2sim 替换为 v2simux
```

## 创建插件
要创建插件，您必须继承抽象类 `PluginBase`。对类名没有特殊要求。必须包含三个基本组件：属性 `Description`、方法 `Init` 和方法 `Work`。我们将先展示代码，然后解释每个函数的用法。
```py
class DemoExternalPlugin(PluginBase):
    @property
    def Description(self) -> str:
        return "此插件的描述"
    
    def Init(self, elem:ET.Element, inst:TrafficInst, work_dir:Path, plg_deps:'List[PluginBase]') -> object:
        self.SetPreStep(self.Work)
        return None

    def Work(self, _t:int, /, sta:PluginStatus) -> Tuple[bool, None]:
        '''插件在时间 _t 的执行函数'''
        raise NotImplementedError
```

+ `Description` 是一个属性，应返回插件的描述。该描述将在仿真开始时显示在命令提示符中，用于简要介绍插件，因此不应过长。
+ `Init` 是插件的核心方法。请注意，您不应定义 Python 类的 `__init__`，因为 `PluginBase` 已将其用于其他目的。此处的 `Init` 函数将在适当的时机由 `PluginBase` 的 `__init__` 调用。让我们看一下它的函数签名：
    + `elem`: 插件描述符的 XML 元素。您可以在您的案例中找到一些 `.plg.xml` 文件，这些文件定义了在该案例中要使用的插件。
    + `inst`: 交通仿真实例。
    + `work_dir`: 工作目录。
    + `plg_deps`: 此插件的依赖项。
+ 在 `Init` 函数中，您应设置插件在不同阶段执行的操作：
    + `self.SetPreSimulation(...)`: 在仿真开始之前、所有其他参数加载之后执行的操作。需要一个 `PINoRet` 类型的函数。
    + `self.SetPreStep(...)`: 在执行每个交通仿真步骤之前执行的操作。需要一个 `PIExec` 类型的函数。
    + `self.SetPostStep(...)`: 在执行每个交通仿真步骤之后执行的操作。需要一个 `PIExec` 类型的函数。
    + `self.SetPostSimulation(...)`: 在仿真结束后执行的操作。需要一个 `PINoRet` 类型的函数。
    + 函数签名 `PIExec`: `def func(time:int, /, status:PluginStatus) -> Tuple[bool, PIResult]`，其中 `bool` 指示插件在此步骤是否成功执行，`PIResult` 是返回值的类型。
    + 函数签名 `PINoRet`: `def func() -> None`

+ `Work` 函数是一个 `PIExec` 函数，用于执行每个步骤的实际工作。它可以通过配置 `Init` 函数在交通步骤之前或之后执行。

## 创建统计项
要创建统计项，您必须继承抽象类 `StaBase`。对类名没有特殊要求。必须包含五个基本组件：属性 `Description`、方法 `__init__`、方法 `GetLocalizedName`、方法 `GetPluginDependency` 和方法 `GetData`。您可以查看我们内部的统计项作为示例，位于 `v2sim.statistics`。

```py
class DemoStatisticItem(StaBase):
    @property
    def Description(self)->str:
        return _L["DESCRIPTION"]
    
    def __init__(self, name:str, path:str, items:List[str], tinst:TrafficInst, 
            plugins:Dict[str,PluginBase], precision:Dict[str, int]={}, compress:bool=True):
        super().__init__(name, path, items, tinst, plugins, precision, compress)
        raise NotImplementedError

    @staticmethod
    def GetLocalizedName() -> str:
        '''获取本地化名称'''
        return _L["STA_NAME"]
    
    @staticmethod
    def GetPluginDependency() -> List[str]:
        '''获取插件依赖'''
        return ["demo"] # 示例：此统计项依赖于 "demo" 插件
    
    def GetData(self, inst:TrafficInst, plugins:Dict[str,PluginBase]) -> Iterable[Any]: 
        '''获取数据'''
        raise NotImplementedError
```

## 导出插件和统计项
为了使插件和统计项能被 V2Sim/V2Sim-UX 识别，您需要设置导出变量：
+ 对于插件，使用 `plugin_exports = (插件名称, 插件类, 插件依赖列表)`，如果没有依赖，使用空列表。如果您不导出插件，请忽略 `plugin_exports`。
+ 对于统计项，使用 `sta_exports = (统计项名称, 统计项类)`。如果您不导出统计项，请忽略 `sta_exports`。

以下是一个合法的示例：
```py
plugin_exports = ("demo", DemoExternalPlugin, ["pdn"])
sta_exports = ("demo", DemoStatisticItem)
```

## 国际化 (i18n)
V2Sim 支持简单的国际化，由 `feasytools` 提供支持。您可以为不同语言定义描述。有两种实现国际化的方式：`单文件` 和 `多文件`。

<!-- tabs:start -->
# **单文件**
假设文件名为 `demo.py`，则必须创建一个名为 `demo.langs` 的语言文件，其内容如下所示：
```
[en_US]
DESCRIPTION=Plugin description
STA_NAME=Demo

[zh_CN]
DESCRIPTION=插件描述
STA_NAME=示例统计项
```

为了加载语言文件，请使用以下 Python 代码：

```py
# 加载语言
from feasytools import LangLib
_L = LangLib.LoadFor(__file__)

# 访问项
print(_L("DESCRIPTION"))
print(_L("STA_DEMO"))
```
# **多文件**
应在您的文件同级目录下创建一个名为 `_lang` 的文件夹。在该文件夹中，存储着各种语言的文件。文件的名称应为 `<语言环境名称>.lang`，例如 `en_US.lang` 和 `zh_CN.lang`。

#### 文件内容
+ `en_US.lang`
```
DESCRIPTION=Plugin description
STA_NAME=Demo
```

+ `zh_CN.lang`
```
DESCRIPTION=插件描述
STA_NAME=示例统计项
```

为了加载语言文件，请使用以下 Python 代码：

```py
# 加载语言
from feasytools import LangLib
_L = LangLib.Load(__file__)

# 访问项
print(_L("DESCRIPTION"))
print(_L("STA_DEMO"))
```
<!-- tabs:end -->

## 导入和移除插件

将您的 Python 文件和语言文件复制到 `~/.v2sim/plugins/`（对于 V2Sim）或 `~/.v2simux/plugins/`（对于 V2Sim-UX）。或者您可以使用启动图形用户界面：点击 "工具" 菜单并选择 "插件..."，然后您可以在弹出的窗口中添加或移除插件。

注：`~`在Windows下通常为`C:\Users\<用户名>`。