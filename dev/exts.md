# Extensions 

In the paper, we have mentioned that many parts can be customized by adding extensions. Now we will show you how to add your own extensions to V2Sim.

An extension is a python file named `ext.py` in a case. When the case is loaded, V2Sim will import the `ext.py` file and execute the code in it. The code in `ext.py` can be used to customize the behavior of V2Sim. An example is given in `cases/std_37nodes/ext.py`. Note that `ext.py` only works for the case where stored, and will not influence other cases.

## AllocEnv
Most extension functions involve a dataclass `AllocEnv`, whose definition is shown below:
```py
@dataclass
class AllocEnv:
    cs: CS            # Related charging station, either FCS or SCS
    EVs: Iterable[EV]   # Related EVs
    CTime: int          # Current time
```

## Customize V2G strategy (V2GAlloc/PdAlloc)
If you want to set the V2G allocation scheme of a charging station to the following function, set the `v2galloc` attribute of the charging station to "MyAverage" in the `*. scs.xml` file.

The input of the V2G allocation function is shown in the function signature. The V2G power of each EV must be set in this function.

Example:

```py
import v2sim
from v2sim import EV, AllocEnv

def MyAverageAllocFunc(env:AllocEnv, veh_cnt: int, v2g_demand: float, v2g_cap: float):
    """
    Customized V2G Strategy
        env: Environment includes SCS, EVs and current time
        veh_cnt: Number of involved EVs
        v2g_demand: V2G power demanded by grid dispatcher
        v2g_cap: Maximum V2G power output of the SCS
    Returns nothing
    """
    if veh_cnt == 0 or v2g_demand == 0: return
    pd = v2g_demand / veh_cnt

    # Important: Must set the actual V2G power of each involved EV by set_temp_pd(...)
    for ev in env.EVs:
        ev.set_temp_pd(pd)

v2sim.V2GAllocPool.add("MyAverage", MyAverageAllocFunc)
```
## Customize charging power allocation strategy (PcAlloc)
Something the charging power supplied by power grid cannot satisfy the demand of the charging EVs. At this time, we have to design a strategy to allocate charging power. The following function is a sample. In order to use it, please set the `pcalloc` attribute of the charging station to "MyAverage" in the `*. fcs.xml` and `*. scs.xml` file.

The input of the charging power allocation function is shown in the function signature. The actual charging power of each EV must be set in this function.

```py
def MyAverageMaxPcAllocator(env: AllocEnv, vcnt:int, max_pc0: float, max_pc_tot: float):
    """
    Average maximum charging power allocator
        env: Allocation environment
        vcnt: Number of vehicles being charged
        max_pc0: Maximum charging power of a single pile, kWh/s
        max_pc_tot: Maximum charging power of the entire CS given by the PDN, kWh/s
    Returns nothing
    """
    if vcnt == 0: return
    pc0 = min(max_pc_tot / vcnt, max_pc0)

    # Important: Must set the actual charging power of each involved EV by set_temp_pc(...)
    for ev in env.EVs:
        ev.set_temp_max_pc(pc0)

v2simux.MaxPCAllocPool.add("MyAverage", MyAverageMaxPcAllocator)
```

## Customize EV battery correction function (BCF)
A battery correction function (BCF) describe the real charging power (Pc) of an EV at a specific situation. For example, the real Pc of an EV at different SOC level may be different, while the nominal Pc is a fixed value. In this case, a BCF is needed.

If you want to set the BCF of a certain electric vehicle to the a function named "MyEquaChargeRate", set the `rmod` attribute of the electric vehicle to "MyEqual" in the `*.veh.xml` file.

The inputof a BCF is shown in the function signature, and its output is the actual charging power.

A BCF example is shown as follows:

```py
import v2sim
from v2sim import EV

def MyEqualChargeRate(rate: float, ev: EV) -> float:
    """
    Charging power modifier
        rate: Nominal charging power
        ev: EV object
    Returns the actual charging power
    """
    return rate

v2sim.ChargeRatePool.add("MyEqual", MyEqualChargeRate)
```