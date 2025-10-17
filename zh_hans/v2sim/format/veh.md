# 车辆与行程格式

车辆描述文件是一个 XML 文件，以名为 `root` 的根元素开头。在根元素中，每个元素名为 `vehicle`，其属性如下：

+ id: 车辆名称
+ soc: 电池荷电状态，即电池电量百分比
+ c: 每米耗电量，单位为 Wh/m
+ rf: 额定快速充电功率，单位为 kW（仿真时将通过 `rmod` 修正为实际充电功率）
+ rs: 额定慢速充电功率，单位为 kW（仿真时将通过 `rmod` 修正为实际充电功率）
+ omega: 描述电动汽车用户单位时间价值的系数
+ kr: 表示电动汽车用户路线长度估计偏差的系数。估计长度为 KRel × 实际长度
+ ks: 触发慢速充电的 SOC 阈值
+ kf: 触发快速充电的 SOC 阈值
+ kv: 触发 V2G 放电的 SOC 阈值
+ eta_c: 充电效率
+ eta_d: 放电效率
+ rmod: 充电功率修正方法
+ cache_route: 是否在仿真过程中缓存路线

`vehicle` 元素中包含多个 `trip` 子元素，其属性如下：
+ id: 行程名称
+ depart: 出发时间，单位为秒
+ fromTaz: 出发交通分区
+ toTaz: 目的交通分区
+ route_edges: 从起点到终点的完整路径边序列，按首尾连接顺序排列。如果只包含 2 个边，则视为起点边和终点边，路线将在仿真过程中自动计算
+ fixed_route: 当 `route_edges` 只包含 2 个边时是否固定路线。由于交通网络是动态的，最优路线可能随时间变化。如果设为 true，将固定为首次为该边对计算的路线

以下是一个合法示例：

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