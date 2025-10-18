# 扩展功能

在论文中我们提到，许多部分可以通过添加扩展来自定义。现在我们将向您展示如何向 V2Sim 添加自己的扩展。

扩展是一个名为 `ext.py` 的 Python 文件，位于案例文件夹中。当案例被加载时，V2Sim 会导入 `ext.py` 文件并执行其中的代码。`ext.py` 中的代码可用于自定义 V2Sim 的行为。示例请参见 `cases/std_37nodes/ext.py`。请注意，`ext.py` 仅对其所在的案例有效，不会影响其他案例。

## AllocEnv
大多数扩展函数都涉及一个数据类 `AllocEnv`，其定义如下：
```py
@dataclass
class AllocEnv:
    cs: CS            # 相关的充电站，可以是 FCS 或 SCS
    EVs: Iterable[EV]   # 相关的电动汽车
    CTime: int          # 当前时间
```

## 自定义 V2G 策略 (V2GAlloc/PdAlloc)
如果您想将充电站的 V2G 分配方案设置为以下函数，请在 `*.scs.xml` 文件中将充电站的 `v2galloc` 属性设置为 "MyAverage"。

V2G 分配函数的名称必须以 `ActualRatio` 结尾，其输入如函数签名所示。必须在此函数中设置每个电动汽车的 V2G 功率。

示例：

```py
import v2sim
from v2sim import EV, AllocEnv

def MyAverageActualRatio(env:AllocEnv, veh_cnt: int, v2g_demand: float, v2g_cap: float):
    """
    自定义 V2G 策略
        env: 包含 SCS、电动汽车和当前时间的环境
        veh_cnt: 涉及的电动汽车数量
        v2g_demand: 电网调度器要求的 V2G 功率
        v2g_cap: SCS 的最大 V2G 功率输出
    无返回值
    """
    if veh_cnt == 0 or v2g_demand == 0: return
    pd = v2g_demand / veh_cnt

    # 重要：必须通过 set_temp_pd(...) 设置每个相关电动汽车的实际 V2G 功率
    for ev in env.EVs:
        ev.set_temp_pd(pd)

v2sim.cs.V2GAllocPool.add("MyAverage", MyAverageActualRatio)
```

## 自定义充电功率分配策略 (PcAlloc)
有时电网提供的充电功率无法满足充电电动汽车的需求。此时，我们需要设计策略来分配充电功率。以下函数是一个示例。要使用它，请在 `*.fcs.xml` 和 `*.scs.xml` 文件中将充电站的 `pcalloc` 属性设置为 "MyAverage"。

充电功率分配函数的名称必须以 `MaxPCAllocator` 结尾，其输入如函数签名所示。必须在此函数中设置每个电动汽车的实际充电功率。

```py
def MyAverageMaxPCAllocator(env: AllocEnv, vcnt:int, max_pc0: float, max_pc_tot: float):
    """
    平均最大充电功率分配器
        env: 分配环境
        vcnt: 正在充电的车辆数量
        max_pc0: 单个充电桩的最大充电功率，单位 kWh/s
        max_pc_tot: 配电网给整个充电站的最大充电功率，单位 kWh/s
    无返回值
    """
    if vcnt == 0: return
    pc0 = min(max_pc_tot / vcnt, max_pc0)

    # 重要：必须通过 set_temp_pc(...) 设置每个相关电动汽车的实际充电功率
    for ev in env.EVs:
        ev.set_temp_max_pc(pc0)
```

## 自定义电池校正函数 (BCF)
电池校正函数(Battery correction function, BCF)描述电动汽车在特定情况下的实际充电功率。例如，电动汽车在不同 SOC 水平下的实际充电功率可能不同，而标称充电功率是固定值。这种情况下就需要 BCF。

如果您想将某辆电动汽车的 BCF 设置为名为 "MyEquaChargeRate" 的函数，请在 `*.veh.xml` 文件中将该电动汽车的 `rmod` 属性设置为 "MyEqual"。

BCF 的名称必须以 `ChargeRate` 结尾。其输入如函数签名所示，输出是实际充电功率。

BCF 示例如下：

```py
import v2sim
from v2sim import EV

def MyEqualChargeRate(rate: float, ev: EV) -> float:
    """
    充电功率修正器
        rate: 标称充电功率
        ev: 电动汽车对象
    返回实际充电功率
    """
    return rate

v2sim.ev.ChargeRatePool.add("MyEqual", MyEqualChargeRate)
```