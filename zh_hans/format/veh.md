# 车辆与行程文件格式

车辆描述文件是一种 XML 文件，其根元素名为 `root`。在根元素中，每个子元素命名为 `vehicle`，其属性如下所列：
+ `id`: 车辆名称
+ `soc`: 荷电状态，即此处电池电量百分比
+ `c`: 每米能耗，单位：瓦时/米
+ `rf`: 额定快充功率，单位：千瓦（在仿真时会被 `rmod` 修正以获得实际充电功率）
+ `rs`: 额定慢充功率，单位：千瓦（在仿真时会被 `rmod` 修正以获得实际充电功率）
+ `omega`: 一个系数，用于描述电动汽车用户的单位时间价值。
+ `kr`: 表示电动汽车用户的路线长度估计偏差。*用户估计长度* 等于 `kr` × *真实行驶距离*。
+ `ks`: 触发慢充的荷电状态阈值。
+ `kf`: 触发快充的荷电状态阈值。
+ `kv`: 触发车对网放电的荷电状态阈值。
+ `eta_c`: 充电效率
+ `eta_d`: 放电效率
+ `rmod`: 充电功率修正方法
+ (仅 V2Sim) `cache_route`: 在仿真期间是否缓存路线

一个 `vehicle` 元素中会有多个 `trip` 子元素，其属性如下所示：
<!-- tabs:start -->

# **V2Sim**
+ `id`: 行程名称
+ `depart`: 出发时间，单位：秒
+ `fromTaz`: 起点交通分析小区
+ `toTaz`: 终点交通分析小区
+ `route_edges`: 从起点路段到终点路段，按首尾连接顺序排列的所有路段。如果只包含 2 个路段，则将其视为起点路段和终点路段，路线将在仿真期间自动查找。
+ `fixed_route`: 如果 `route_edges` 中只包含 2 个路段，是否固定路线。由于交通网络是动态的，最优路线在不同时间可能不同。如果 `fixed_route` 为 true，则将其固定为首次为该路段对找到的路线。（**注意**：`fixed_route` 存在错误，可能无法按预期工作！）

# **V2Sim-UX**
+ `id`: 行程名称
+ `depart`: 出发时间，单位：秒
+ `fromNode`: 起点节点
+ `toNode`: 终点节点

允许为了您自己的目的添加额外属性，但它们不会被 V2Sim-UX 使用。

<!-- tabs:end -->

一个合法的示例如下所示：
<!-- tabs:start -->

# **V2Sim**
```xml
<root>
<vehicle id="v0" soc="0.8000" bcap="76.8000" c="0.17066668" rf="100.0000" rs="7.0000" rv="20.0000" omega="7.102858"
  kf="0.2202" ks="0.5023" kv="0.6803" kr="1.0518" eta_c="0.9" eta_d="0.9" rmod="Linear" cache_route="False">
<trip id="trip0_1" depart="35640" fromTaz="TAZ4" toTaz="TAZ10" route_edges="gneE35 gneE26" fixed_route="None" />
<trip id="trip0_2" depart="779340" fromTaz="TAZ10" toTaz="TAZ8" route_edges="gneE26 gneE42" fixed_route="None" />
<trip id="trip0_3" depart="866040" fromTaz="TAZ8" toTaz="TAZ4" route_edges="gneE42 gneE35" fixed_route="None" />
<trip id="trip1_1" depart="120420" fromTaz="TAZ4" toTaz="TAZ3" route_edges="gneE35 gneE23" fixed_route="None" />
<trip id="trip1_2" depart="162120" fromTaz="TAZ3" toTaz="TAZ8" route_edges="gneE23 gneE42" fixed_route="None" />
<trip id="trip1_3" depart="175920" fromTaz="TAZ8" toTaz="TAZ4" route_edges="gneE42 gneE35" fixed_route="None" />
<!--更多行程-->
</vehicle>
<!--更多车辆-->
</root>
```

# **V2Sim-UX**
```xml
<root>
<vehicle id="v0" soc="0.8000" bcap="76.8000" c="0.17066667" rf="100.0000" rs="7.0000" rv="20.0000" omega="7.102858"
  kf="0.2202" ks="0.5023" kv="0.7284" kr="1.0518" eta_c="0.9" eta_d="0.9" rmod="Linear">
<trip id="trip0_1" depart="22380" fromType="Home" toType="Work" fromNode="T4" toNode="T6" />
<trip id="trip0_2" depart="70380" fromType="Work" toType="Relax" fromNode="T6" toNode="T12" />
<trip id="trip0_3" depart="77880" fromType="Relax" toType="Home" fromNode="T12" toNode="T4" />
<trip id="trip1_1" depart="127980" fromType="Home" toType="Relax" fromNode="T4" toNode="T8" />
<trip id="trip1_2" depart="131880" fromType="Relax" toType="Home" fromNode="T8" toNode="T1" />
<trip id="trip1_3" depart="145680" fromType="Home" toType="Home" fromNode="T1" toNode="T4" />
<!--更多行程-->
</vehicle>
<!--更多车辆-->
</root>
```

<!-- tabs:end -->