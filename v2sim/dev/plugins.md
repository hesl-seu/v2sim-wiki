You can add plugins and statistics to V2Sim!

### Plugin
A plugin can be used by different cases. It can be used to customize the behavior of V2Sim, or to add new features to V2Sim. 

Place the plugin file in `/external_components/`. The file name must be same as the plugin name. For example, if your plugin is called `demo`, then the file name must be `demo.py`.

A plugin must inherit the class `PluginBase` defined in `v2sim/plugins/base.py`. Method `Init` and `Work` are necessary. The former is used to initialize the plugin, and the latter is used to run the plugin. Property `Description` must be also included. The following is an example of a plugin class:

```py
from v2sim.plugins import *

class DemoExternalPlugin(PluginBase):
    @property
    def Description(self)->str:
        return "A demo plugin"
    
    def Init(self,elem:ET.Element,inst:TrafficInst,work_dir:Path,plg_deps:'list[PluginBase]') -> object:
        '''
        Add plugin initialization code here, return:
            Return value when the plugin is offline
        '''
        self.SetPreStep(self.Work)
        return None

    def Work(self,_t:int,/,sta:PluginStatus)->tuple[bool,None]:
        '''The execution function of the plugin at time _t'''
        raise NotImplementedError
```

Finally, export the plugin:

```py
plugin_exports = ("demo", DemoExternalPlugin, ["pdn"])
```

Here `demo` is the name of the plugin, `DemoExternalPlugin` is the class name, and `["pdn"]` is the list of dependencies. If the plugin has no dependencies, just use `[]`.