# 算例扩展

在论文中，我们提到可以通过添加扩展来自定义许多部件。现在我们将展示如何向 V2Sim 添加您自己的扩展。

扩展是一个位于案例中的名为`ext.py`的 Python 文件。当案例被加载时，V2Sim 会导入`ext.py`文件并执行其中的代码。`ext.py`中的代码可用于自定义V2Sim的行为。示例请参见 `cases/std_37nodes/ext.py`。需要注意的是，由于`ext.py`存储在特定算例文件中，因此不会对其他算例产生效果。

## 自定义V2G策略

如果您想将充电站的 V2G 分配方案设置为以下函数，请在`*.scs.xml`文件中将充电站的`v2galloc`属性设置为 "MyAverage"。

V2G 分配函数的名称必须以`ActualRatio`结尾，其输入如函数签名所示。输出是 ev_ids 列表中每辆电动汽车的放电功率乘数，其顺序需要与 ev_ids 保持一致。

示例：

```py
import v2sim
from v2sim import EV, AllocEnv

def MyAverageActualRatio(env: 'AllocEnv', v2g_k: 'float') -> 'list[float]':
    return len(env.EVs) * [v2g_k]

v2sim.cs.V2GAllocPool.add("MyAverage", MyAverageActualRatio)
```

## 自定义EV电池校正函数
电池校正函数(Battery correction function, BCF)描述了电动汽车在特定情况下的真实充电功率。例如，电动汽车在不同 SOC 水平下的真实充电功率可能不同，而额定充电功率是一个固定值。在这种情况下，就需要BCF刻画真实功率。

如果您想将某辆电动汽车的BCF设置为名为`MyEqualChargeRate`的函数，请在`*.veh.xml`文件中将该电动汽车的`rmod`属性设置为`MyEqual`。

BCF的名称必须以`ChargeRate`结尾。其输入如函数签名所示，输出是实际充电功率。

BCF示例如下：
```py
import v2sim
from v2sim import EV

def MyEqualChargeRate(rate: float, ev: 'EV') -> float:
    return rate

v2sim.ev.ChargeRatePool.add("MyEqual", MyEqualChargeRate)
```