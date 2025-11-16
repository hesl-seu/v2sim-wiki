**扩展功能**

在论文中，我们提到许多部分可以通过添加扩展来自定义。现在我们将向您展示如何向 V2Sim 添加您自己的扩展。

扩展是一个位于案例目录中名为 `ext.py` 的 Python 文件。当案例被加载时，V2Sim 将导入 `ext.py` 文件并执行其中的代码。`ext.py` 中的代码可用于自定义 V2Sim 的行为。一个示例在 `cases/std_37nodes/ext.py` 中给出。请注意，`ext.py` 仅对其所在的案例有效，不会影响其他案例。

## 分配环境
大多数扩展函数涉及一个数据类 `AllocEnv`，其定义如下所示：
```py
@dataclass
class AllocEnv:
    cs: CS            # 相关的充电站，可以是快充站或慢充站
    EVs: Iterable[EV]   # 相关的电动汽车
    CTime: int          # 当前时间
```

## 自定义车对网策略 (V2G分配/放电分配)
如果您想将充电站的车对网分配方案设置为以下函数，请在 `*.scs.xml` 文件中将充电站的 `v2galloc` 属性设置为 "MyAverage"。

车对网分配函数的输入如函数签名所示。必须在此函数中设置每辆电动汽车的车对网放电功率。

示例：

```py
import v2sim
from v2sim import EV, AllocEnv

def MyAverageAllocFunc(env:AllocEnv, veh_cnt: int, v2g_demand: float, v2g_cap: float):
    """
    自定义车对网策略
        env: 包含慢充站、电动汽车和当前时间的环境信息
        veh_cnt: 涉及的电动汽车数量
        v2g_demand: 电网调度员要求的车对网放电功率
        v2g_cap: 该慢充站的最大车对网放电功率输出能力
    无返回值
    """
    if veh_cnt == 0 or v2g_demand == 0: return
    pd = v2g_demand / veh_cnt

    # 重要：必须通过 set_temp_pd(...) 方法为每辆涉及的电动汽车设置实际的车对网放电功率
    for ev in env.EVs:
        ev.set_temp_pd(pd)

v2sim.V2GAllocPool.add("MyAverage", MyAverageAllocFunc)
```

## 自定义充电功率分配策略 (充电功率分配)
有时电网提供的充电功率无法满足正在充电的电动汽车的需求。此时，我们需要设计一种策略来分配充电功率。以下函数是一个示例。要使用它，请在 `*.fcs.xml` 和 `*.scs.xml` 文件中将充电站的 `pcalloc` 属性设置为 "MyAverage"。

充电功率分配函数的输入如函数签名所示。必须在此函数中设置每辆电动汽车的实际充电功率。

```py
def MyAverageMaxPcAllocator(env: AllocEnv, vcnt:int, max_pc0: float, max_pc_tot: float):
    """
    平均最大充电功率分配器
        env: 分配环境
        vcnt: 正在充电的车辆数量
        max_pc0: 单个充电桩的最大充电功率，单位：千瓦时/秒
        max_pc_tot: 配电网给予整个充电站的最大充电功率，单位：千瓦时/秒
    无返回值
    """
    if vcnt == 0: return
    pc0 = min(max_pc_tot / vcnt, max_pc0)

    # 重要：必须通过 set_temp_pc(...) 方法为每辆涉及的电动汽车设置实际充电功率
    for ev in env.EVs:
        ev.set_temp_max_pc(pc0)

v2sim.MaxPCAllocPool.add("MyAverage", MyAverageMaxPcAllocator)
```

## 自定义电动汽车电池修正函数 (电池修正函数)
电池修正函数描述了电动汽车在特定情况下的实际充电功率。例如，电动汽车在不同荷电状态水平下的实际充电功率可能不同，而额定充电功率是一个固定值。在这种情况下，就需要电池修正函数。

如果您想将某辆电动汽车的电池修正函数设置为名为 "MyEqualChargeRate" 的函数，请在 `*.veh.xml` 文件中将该电动汽车的 `rmod` 属性设置为 "MyEqual"。

电池修正函数的输入如函数签名所示，其输出是实际充电功率。

电池修正函数示例如下：

```py
import v2sim
from v2sim import EV

def MyEqualChargeRate(rate: float, ev: EV) -> float:
    """
    充电功率修正器
        rate: 额定充电功率
        ev: 电动汽车对象
    返回实际充电功率
    """
    return rate

v2sim.ChargeRatePool.add("MyEqual", MyEqualChargeRate)
```