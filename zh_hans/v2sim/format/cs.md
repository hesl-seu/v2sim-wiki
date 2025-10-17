# 充电站格式

充电站描述文件是一个 XML 文件，以名为 `root` 的根元素开头。在根元素中，每个元素名为 `fcs`（快速充电站）或 `scs`（慢速充电站），其属性如下：

+ name: 充电站名称，应与道路名称相同
+ edge: 充电站所在的道路
+ slots: 充电站中的充电桩数量
+ bus: 充电站连接的电网总线
+ x, y: 充电站位置坐标
+ max_pc: 最大充电功率，单位为 kW
+ max_pd: 最大放电功率（V2G），单位为 kW
+ pc_alloc: 为充电站中每辆电动汽车分配充电负荷削减的方法
+ pd_alloc: 为充电站中每辆电动汽车分配放电负荷的方法

充电站元素还可能包含两个用于定价的子元素：
+ pbuy: 充电价格，FCS 和 SCS 都必须设置
+ psell: 放电价格，SCS 可选。FCS 不支持此功能

以下是一个合法的示例：

```xml
<root>
  <scs name="edge10_1" edge="edge10_1" slots="10" bus="B17" x="inf" y="inf" max_pc="200.00" max_pd="1000.00" pc_alloc="Average" pd_alloc="Average">
  <!--edge 在编辑器中显示而 name 不显示。两者必须相同。-->
  <!--max_pc 和 max_pd 的单位是 kW。-->
    <pbuy>
      <item btime="0" price="1.5"/>
      <item btime="3600" price="1.4"/>
    </pbuy>
    <psell>
      <item btime="0" price="1.0"/>
      <item btime="3600" price="2.0"/>
    </psell>
  </scs>
  <!--如需描述 FCS，请将 scs 替换为 fcs。-->
</root>
```