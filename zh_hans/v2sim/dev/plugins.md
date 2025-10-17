# 为 V2Sim 添加插件和统计功能

### 插件

插件可以被不同的案例使用。它可用于自定义 V2Sim 的行为，或为 V2Sim 添加新功能。

将插件文件放置在 `/external_components/` 目录下。文件名必须与插件名相同。例如，如果您的插件名为 `demo`，那么文件名必须是 `demo.py`。

插件必须继承 `v2sim/plugins/base.py` 中定义的 `PluginBase` 类。必须实现 `Init` 和 `Work` 方法。前者用于初始化插件，后者用于运行插件。还必须包含 `Description` 属性。以下是一个插件类的示例：

```py
from v2sim.plugins import *

class DemoExternalPlugin(PluginBase):
    @property
    def Description(self)->str:
        return "一个演示插件"
    
    def Init(self,elem:ET.Element,inst:TrafficInst,work_dir:Path,plg_deps:'list[PluginBase]') -> object:
        '''
        在此添加插件初始化代码，返回：
            插件离线时的返回值
        '''
        self.SetPreStep(self.Work)
        return None

    def Work(self,_t:int,/,sta:PluginStatus)->tuple[bool,None]:
        '''插件在时间 _t 的执行函数'''
        raise NotImplementedError
```

最后，导出插件：

```py
plugin_exports = ("demo", DemoExternalPlugin, ["pdn"])
```
这里`demo`是插件名称，`DemoExternalPlugin`是类名，`["pdn"]`是依赖项列表。如果插件没有依赖项，只需使用`[]`。
