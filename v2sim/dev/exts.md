# Extensions 

In the paper, we have mentioned that many parts can be customized by adding extensions. Now we will show you how to add your own extensions to V2Sim.

An extension is a python file named `ext.py` in a case. When the case is loaded, V2Sim will import the `ext.py` file and execute the code in it. The code in `ext.py` can be used to customize the behavior of V2Sim. An example is given in `cases/std_37nodes/ext.py`. Note that `ext.py` only works for the case where stored, and will not influence other cases.

## AllocEnv
Most extension functions involve a dataclass `AllocEnv`, whose definition is shown below:
```py
@dataclass
class AllocEnv:
    cs: 'CS'            # Related charging station, either FCS or SCS
    EVs: Iterable[EV]   # Related EVs
    CTime: int          # Current time
```

## Customize V2G strategy
If you want to set the V2G allocation scheme of a charging station to the following function, set the `v2galloc` attribute of the charging station to "MyAverage" in the `*. scs.xml` file.

The name of the V2G allocation function must end with `ActualRatio`, and its input is as shown in the function signature. The output is the discharge power multiplier for each vehicle in the ev_ids list, which needs to be kept in the same order as ev_ids.

Example:

```py
import v2sim
from v2sim import EV, AllocEnv

def MyAverageActualRatio(env: 'AllocEnv', v2g_k: 'float') -> 'list[float]':
    return len(env.EVs) * [v2g_k]

v2sim.cs.V2GAllocPool.add("MyAverage", MyAverageActualRatio)
```

## Customize EV battery correction function (BCF)
A battery correction function (BCF) describe the real charging power (Pc) of an EV at a specific situation. For example, the real Pc of an EV at different SOC level may be different, while the nominal Pc is a fixed value. In this case, a BCF is needed.

If you want to set the BCF of a certain electric vehicle to the a function named "MyEquaChargeRate", set the `rmod` attribute of the electric vehicle to "MyEqual" in the `*.veh.xml` file.

The name of a BCF must end with `ChargeRate`. Its input is as shown in the function signature, and its output is the actual charging power.

A BCF example is shown as follows:

```py
import v2sim
from v2sim import EV

def MyEqualChargeRate(rate: float, ev: 'EV') -> float:
    return rate

v2sim.ev.ChargeRatePool.add("MyEqual", MyEqualChargeRate)
```