# CS Format
A CS description file is a xml file starting with a root element named `root`. In the root element, each element is named either `fcs` (fast charing station) or `scs` (slow charing station), whose attributes are listed below:

+ name: name of the CS, should be the same as the edge
+ edge: the edge at which the CS located
+ slots: number of chargers in the CS
+ bus: the bus to which the CS connected
+ x, y: position of the CS
+ max_pc: maximum charging power, in kW
+ max_pd: maximum discharging power (V2G), in kW
+ pc_alloc: method to allocate charging load reduction to each EV in the CS
+ pd_alloc: method to allocate discharging load to each EV in the CS

A CS element also has two possible child elements for pricing:
+ pbuy: price for charging, complusory for both FCS and SCS.
+ psell: price for discharging, optional for SCS. FCS does not support this feature.

A legal example is shown below:

```xml
<root>
  <scs name="edge10_1" edge="edge10_1" slots="10" bus="B17" x="inf" y="inf" max_pc="200.00" max_pd="1000.00" pc_alloc="Average" pd_alloc="Average">
  <!--edge is shown in the editor while name is not. They must be the same.-->
  <!--max_pc and max_pd are in kW.-->
    <pbuy>
      <item btime="0" price="1.5"/>
      <item btime="3600" price="1.4"/>
    </pbuy>
    <psell>
      <item btime="0" price="1.0"/>
      <item btime="3600" price="2.0"/>
    </psell>
  </scs>
  <!--Replace scs with fcs if you want to describe a FCS.-->
</root>
```