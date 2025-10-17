# Vehicle and trips format
A vehicle description file is a xml file starting with a root element named `root`. In the root element, each element is named `vehicle`, whose attributes are listed below:
+ id: name of the vehicle
+ soc: state of charge, i.e. percentage of electricity in the battery here
+ c: electricity consumed per meter, Wh/m
+ rf: nominal fast charging power, kWh (will be modified by `rmod` to get the real charing power when simulation)
+ rs: nominal slow charging power, kWh (will be modified by `rmod` to get the real charing power when simulation)
+ omega: A coefficient to describe the value of unit time of the EV user.
+ kr: It symbols the route length estimation offset of the EV user. The estimated length is KRel × real length.
+ ks: SoC threshold for slow charging.
+ kf: Soc threshold for fast charging.
+ kv: Soc threshold for V2G dischariging.
+ eta_c: charging efficiency
+ eta_d: discharging efficiency
+ rmod: modification method of charing power
+ cahce_route: whether to cache the route during simulation

There will be multiple `trip` child elements in a `vehicle` element, whose attributes are shown below:
+ id: name of the trip
+ depart: time to depart, in second
+ fromTaz: source TAZ
+ toTaz: destination TAZ
+ route_edges: all the edges in the route from source edge to destination edge in head-tail-connected order. If only 2 edges are included, it will be regared as the source edge and the destination edge, route will be automatically found during simulation.
+ fixed_route: whether to fix the route if only 2 edges are included in `route_edges`. Since transportation network is dynamic, the optimal route may be different at different time. If `fixed_route` is true, it will be fixed as the route that found for first time for the edge pair.

A legal example is shown below:
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
<!--more trips-->
</vehicle>
<!--more vehicles-->
</root>
```