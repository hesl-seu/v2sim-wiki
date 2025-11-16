# 充电站文件格式

充电站描述文件是一种 XML 文件，其根元素名为 `root`。在根元素中，每个子元素要么命名为 `fcs`（快充站），要么命名为 `scs`（慢充站），其属性如下所列：

+ `name`: 充电站的名称，应与边缘（edge）名称相同
+ `edge`: 充电站所在的道路边缘
+ `slots`: 充电站中的充电桩数量
+ `bus`: 充电站连接的电网总线
+ `x`, `y`: 充电站的位置坐标
+ `max_pc`: 最大充电功率，单位为千瓦
+ `max_pd`: 最大放电功率（车对网），单位为千瓦
+ `pc_alloc`: 将充电负荷削减分配给充电站内各电动汽车的方法
+ `pd_alloc`: 将放电负荷分配给充电站内各电动汽车的方法

充电站元素还可以包含两个用于定价的可选子元素：
+ pbuy: 充电价格，对于快充站和慢充站都是必需的。
+ psell: 放电价格，对于慢充站是可选的。快充站不支持此功能。

一个合法的示例如下所示：

```xml
<root>
  <scs name="edge10_1" edge="edge10_1" slots="10" bus="B17" x="inf" y="inf" max_pc="200.00" max_pd="1000.00" pc_alloc="Average" pd_alloc="Average">
  <!--edge 在编辑器中显示，而 name 不显示。它们必须相同。-->
  <!--max_pc 和 max_pd 的单位是千瓦。-->
    <pbuy>
      <item btime="0" price="1.5"/>
      <item btime="3600" price="1.4"/>
    </pbuy>
    <psell>
      <item btime="0" price="1.0"/>
      <item btime="3600" price="2.0"/>
    </psell>
  </scs>
  <!--如果您想描述一个快充站，请将 scs 替换为 fcs。-->
</root>
```