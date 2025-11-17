# Plugins and Statistics
You can add plugins and statistics to V2Sim!

A plugin can be used by different cases. It can be used to customize the behavior of V2Sim, or to add new features to V2Sim. 

Place the plugin file in `/external_components/`. The file name must be same as the plugin name. For example, if your plugin is called `demo`, then the file name must be `demo.py`. **Please note that your plugin must not share the same name as any other package installed; otherwise it could not be imported correctly.**

A plugin must inherit the class `PluginBase` defined in `v2sim/plugins/base.py`. Method `Init` and `Work` are necessary. The former is used to initialize the plugin, and the latter is used to run the plugin. Property `Description` must be also included. The following is an example of a plugin class:

## Import necessary packages
To construct plugins and statistics, please import the following packages:
```py
import xml.etree.ElementTree as ET
from pathlib import Path
from typing import Iterable, Any, Tuple, List, Dict
from feasytools import LangLib # for multi-languages
from v2sim import CustomLocaleLib, TrafficInst
from v2sim.plugins import *
from v2sim.statistics import *
# Replace v2sim with v2simux if you are using V2Sim-UX
```

## Create a Plugin
To create a plugin, you have to inherit from the abstract class `PluginBase`. There is no special demand for the class name. Three basic components must be included, property `Description`, method `Init`, and method `Work`. We will present you the code first, and later explain the usage of each function.
```py
class DemoExternalPlugin(PluginBase):
    @property
    def Description(self) -> str:
        return "Description of this plugin"
    
    def Init(self, elem:ET.Element, inst:TrafficInst, work_dir:Path, plg_deps:'List[PluginBase]') -> object:
        self.SetPreStep(self.Work)
        return None

    def Work(self, _t:int, /, sta:PluginStatus) -> Tuple[bool, None]:
        '''The execution function of the plugin at time _t'''
        raise NotImplementedError
```

+ `Description` is a property, which should return the description of the plugin. The description will be displayed in the command prompt when simulation starts to briefly introduce the plugin, so it should not be too long.
+ `Init` is a core method of a plugin. Note that you should not define `__init__` of Python class because `PluginBase` has occupied it for other purposes. `Init` function here will be called at proper opportune by `PluginBase`'s `__init__`. Let's take a look at its function signature:
    + `elem`: XML element of the plugin descriptor. You may find some `.plg.xml` file in your case, which defines the plugin to be used in that case.
    + `inst`: The traffic instance.
    + `work_dir`: Working directory.
    + `plg_deps`: Depenedency of this plugin.
+ In the `Init` function, you should set what the plugin does at different stages:
    + `self.SetPreSimulation(...)`: Things done before the simualtion starts and after all other parameters are loaded. Require a `PINoRet`.
    + `self.SetPreStep(...)`: Things done before each traffic simulation step is performed. Require a `PIExec`.
    + `self.SetPreStep(...)`: Things done after each traffic simulation step is performed. Require a `PIExec`.
    + `self.SetPostSimulation(...)`: Things done after the simulation ended. Require a `PINoRet`.
    + Function signature `PIExec`: `def func(time:int, /, status:PluginStatus) -> Tuple[bool, PIResult]`, where the `bool` indicates whether the plugin is executed successfully at this step, and `PIResult` is the type of return value.
    + Function signature `PINoRet`: `def func() -> None`

+ `Work` function is a `PIExec` function to conduct the actual work of each step. It can be executed either before or after a traffic step, by configuring the `Init` function.

## Create a Statistic Item
To create a statistic item, you have to inherit from the abstract class `StaBase`. There is no special demand for the class name. Three basic components must be included, property `Description`, method `__init__`, method `GetLocalizedName`, method `GetPluginDependency`, and method `GetData`. You can take a look at our internal statistic items as examples, at `v2sim.statistics`.

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
        '''Get Localized Name'''
        return _L["STA_NAME"]
    
    @staticmethod
    def GetPluginDependency() -> List[str]:
        '''Get Plugin Dependency'''
        return ["demo"] # Example: This statistic item depends on the "demo" plugin
    
    def GetData(self, inst:TrafficInst, plugins:Dict[str,PluginBase]) -> Iterable[Any]: 
        '''Get Data'''
        raise NotImplementedError
```

## Export plugins and statistics
To make the plugins and statistics can be recognized by V2Sim/V2Sim-UX, you need to set export variables:
+ For plugins, use `plugin_exports = (Plugin name, Plugin class, Plugin dependency list)`, if there is no dependency, use an empty list. If you don't export a plugin, please ignore `plugin_exports`.
+ For statistics, use `sta_exports = (Statistic item name, Statistic item class)`. If you don't export a statistic item, please ignore `sta_exports`.

Here is a legal example:
```py
plugin_exports = ("demo", DemoExternalPlugin, ["pdn"])
sta_exports = ("demo", DemoStatisticItem)
```

## Internationalization (i18n)
V2Sim supports simple i18n, which is powered by `feasytools`. You may define the description for different languages. There are two ways to realize i18n: `Single File` and `Multiple File`.

<!-- tabs:start --->
# **Single File**
Assuming that the file is named `demo.py`, a language file named `demo.langs` must be created, whose content is shown below:
```
[en_US]
DESCRIPTION=Plugin description
STA_NAME=Demo

[zh_CN]
DESCRIPTION=插件描述
STA_NAME=示例统计项
```

In order to load the language file, use the following Python code:

```py
# Load languages
from feasytools import LangLib
_L = LangLib.LoadFor(__file__)

# Access the items
print(_L("DESCRIPTION"))
print(_L("STA_DEMO"))
```
# **Multiple File**
A folder named `_lang` should be created at the same level of your file. In the folder, files of languages are stored. The name of the files should be `<Locale name>.lang`, such as `en_US.lang` and `zh_CN.lang`.

#### File contents
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

In order to load the language file, use the following Python code:

```py
# Load languages
from feasytools import LangLib
_L = LangLib.Load(__file__)

# Access the items
print(_L("DESCRIPTION"))
print(_L("STA_DEMO"))
```
<!-- tabs:end -->

## Import and Remove Plugins
Copy your Python file and language file to `~/.v2sim/plugins/` (for V2Sim) or `~/.v2simux/plugins/` (for V2Sim-UX). Or you can use the start-up GUI: Click the "Tool" menu and select "Plugins...", then you can add or remove plugins in the pop up window.

Note: `~` is usually `C:\Users\<username>` in Windows.