# PDN file format

**中文版 Chinese version:** [FPowerKit PDN file format](https://gitee.com/fmy_xfk/fpowerkit/blob/master/docs/xml_file.md)

PDN file follows the XML format. A PDN file looks like the following lines:
```xml
<grid Sb="..." Ub="...">
  <bus ID="...">
    <Pd>...</Pd>
    <Qd>...</Qd>
  </bus>
  <line ID="..." From="..." To="..." R="..." X="..."/>
  <gen ID="..." Bus="...">
    <Pmin>...</Pmin>
    <Qmin>...</Qmin>
    <Pmax>...</Pmax>
    <Qmax>...</Qmax>
    <CostA>...</CostA>
    <CostB>...</CostB>
    <CostC>...</CostC>
  </gen>
</grid>
```

## 1. Root element: `grid`
A PDN file starts by a `<grid>` root element. It must contain two attributes: `Sb` and `Ub`, representing base power and base voltage repectively. The attributes must include the unit. For `Sb`, `MVA` and `kVA` are supported; for `Ub`, only `kV` is supported. Note that the unit is case-sensitive.

Here are 2 legal examples:
```xml
<grid Sb="1MVA" Ub="10.0kV">...</grid>
<grid Sb="500.0kVA" Ub="10.0kV">...</grid>
```

**If you want to create IEEE33 or IEEE69 network, please go to section 6.**

## 2. Bus element: `bus`
A `<bus>` element symbols a bus, where bus name and bus load are required. Bus name is assigned by attribute `ID`, while the the bus load is assigned by child elements `<Pd>` and `<Qd>`.

### 2.1 Fixed load
The load of a bus can be either fixed or variable. Fixed load, such as 2.5+j1.0MVA, can be described by the following lines:
```xml
<bus ID="bus_name">
  <Pd const="2.5MW" />
  <Qd const="1.0Mvar" />
</bus>
```
Fixed load is denoted by attribute `const`, supporting unit of `MW`, `kW` (active power), `Mvar`, `kvar`(reactive power), and`pu`. Here `pu` symbols per unit value. In the fixed load case, child elements are forbidden for Pd and Qd elements.

Another legal example for fixed load:
```xml
<bus ID="bus_name">
  <Pd const="2.5" unit="MW" />
  <Qd const="1.0" unit="Mvar" />
</bus>
```
where the value and its unit are separated.

### 2.2 Segment function load
When the load of a bus fluctuates with time, such as 2.5+j1.0MVA since 0s and 3.0+j1.2MVA since 3600s, it should be described by the following lines:
```xml
<bus ID="bus_name">
  <Pd>
    <item time="0" value="2.5MW" />
    <item time="3600" value="3.0MW" />
  </Pd>
  <Qd>
    <item time="0" value="1.0Mvar" />
    <item time="3600" value="1.2Mvar" />
  </Qd>
</bus>
```

Segment function load can be represented by child element `<item>`, where attribute `time` denotes the start time while attribute `value` denotes the value. The unit is the same as the fixed load. Multiple `<item>` means multiple segments.

To represent periodic load, attributes `repeat` and `period` may be added in the element `<Pd>` and `<Qd>`. Example:

```xml
<bus ID="bus_name">
  <Pd repeat="2" period="7200">
    <item time="0" value="2.5MW" />
    <item time="3600" value="3.0MW" />
  </Pd>
  <Qd repeat="2" period="7200">
    <item time="0" value="1.0Mvar" />
    <item time="3600" value="1.2Mvar" />
  </Qd>
</bus>
```

which is equivalent to:

```xml
<bus ID="bus_name">
  <Pd>
    <item time="0" value="2.5MW" />
    <item time="3600" value="3.0MW" />
    <item time="7200" value="2.5MW" />
    <item time="10800" value="3.0MW" />
  </Pd>
  <Qd>
    <item time="0" value="1.0Mvar" />
    <item time="3600" value="1.2Mvar" />
    <item time="7200" value="1.0Mvar" />
    <item time="10800" value="1.2Mvar" />
  </Qd>
</bus>
```

Please note that periodic load does not support separated value and unit.

## 3. Line element: `line`
`<line>` element is used to represent a transmission line. For a line, its name (attribute `ID`), source bus (attribute `From`), terminal bus (attribute `To`), resistance (attribute `R`) and reactance (attribute `X`) are required, where the unit of R and X is always **Ohm**.

Here is a legal example:
```xml
<line ID="line1" From="bus1" To="bus2" R="0.04ohm" X="0.002ohm" />
```

## 4. Generator element: `gen`
`<gen>` or `<generator>` element is used to represent a generator, whose name (attribute `ID`) and bus (attribute `bus`) are required. Limit of generation (attribute `Pmin`, `Pmax`, `Qmin`, `Qmax`) is also required, in the form of `Pd` or `Qd` attribute for a bus. The coefficients for generation cost (fixed cost, primary cost, and secondary cost) are also required, denoted in child elements `<CostA>`, `<CostB>`, and `<CostC>`.

The cost coefficient also provides 2 forms of representation: fixed values and segment functions, similar to `Pd` and `Qd`. However, the units of the cost coefficient are different. For secondary costs, the supported units are `$/kWh2`, `$/MWh2`, and `$/puh2`; For one-time costs, the supported units are `$/kWh`, `$/MWh`, and `$/puh`; For fixed costs, the supported units are `$`, but since there is only one available unit, it can be omitted.

Here is a legal example:
```xml
<gen ID="G1" Bus="B1">
  <Pmin const="0.0MW" />
  <Qmin const="-30.0Mvar" />
  <Pmax const="30MW" />
  <Qmax const="30Mvar" />
  <CostA unit="$/kWh2" const="0.0001" />
  <CostB unit="$/kWh" const="0.3" />
  <CostC const="10.0" />
</gen>
```

## 5. Generator model element: `genmodel`
Considering that there may be multiple generators of the same model in the distribution network, a generator model element `<genmodel>` (or `<generatormodel>`) is set up to reduce the input. The generator model is consistent with the generator node except that it does not require the 'ID' and 'bus' attributes. In addition, the 'name' attribute needs to be provided to represent the model name. When using a generator model, the 'model' attribute must be added to specify the model used.

Here is a legal example:
```xml
<genmodel name="default">
  <Pmin const="0.0MW" />
  <Qmin const="-30.0Mvar" />
  <Pmax const="30MW" />
  <Qmax const="30Mvar" />
  <CostA unit="$/kWh2" const="0.0001" />
  <CostB unit="$/kWh" const="0.3" />
  <CostC const="10.0" />
</genmodel>
<gen ID="G1" Bus="B1" model="default" />
<gen ID="G2" Bus="B2" model="default" />
```

which is equivalent to:
```xml
<gen ID="G1" Bus="B1">
  <Pmin const="0.0MW" />
  <Qmin const="-30.0Mvar" />
  <Pmax const="30MW" />
  <Qmax const="30Mvar" />
  <CostA unit="$/kWh2" const="0.0001" />
  <CostB unit="$/kWh" const="0.3" />
  <CostC const="10.0" />
</gen>
<gen ID="G2" Bus="B2">
  <Pmin const="0.0MW" />
  <Qmin const="-30.0Mvar" />
  <Pmax const="30MW" />
  <Qmax const="30Mvar" />
  <CostA unit="$/kWh2" const="0.0001" />
  <CostB unit="$/kWh" const="0.3" />
  <CostC const="10.0" />
</gen>
```

## 6. Energy storage system element: `ess`
A energy storage system (ESS) looks like the following lines:
```xml
<ess ID="ESS10" Bus="B10" cap="1MWh" ec="0.9" ed="0.9" 
    pc_max="100kW" pd_max="100kW" pf="0.95" init_elec="0.1MWh"
    policy="2" cprice="0.8$/kWh" dprice="1.2$/kWh" />
```
where
+ ID: name of the ESS
+ Bus: to which bus the ESS connects
+ cap: Capacity
+ ec: Charging efficiency, 0~1
+ ed: Discharging efficiency, 0~1
+ pc_max: Maximum charging power
+ pd_max: Maximum discharging pwoer
+ pf: Power factor
+ init_elec: Initial electricity stored in the ESS, 0 < init_elec < cap
+ policy: charging and discharging policy
  - policy = 2: by price, controlled by `cprice` and `dprice`
     - cprice: charging when price is strictly lower than this price
     - dprice: discharging when price is strictyly higher than this price
  - policy = 1: by time, controlled by `ctime` and `dtime`
     - ctime: charging when in the given range
     - dtime: discharging when in the given range

When policy = 2, `cprice` and `dprice` elements are required in the root element. Example:
```xml
<cprice repeat="8" period="86400">
  <item time="0" value="1.0$/kWh" />
  <item time="7200" value="0.7$/kWh" />
  <item time="21600" value="1$/kWh" />
  <item time="25200" value="1.3$/kWh" />
  <item time="32400" value="1$/kWh" />
  <item time="36000" value="0.7$/kWh" />
  <item time="38600" value="0.4$/kWh" />
  <item time="50400" value="0.7$/kWh" />
  <item time="54000" value="1.0$/kWh" />
  <item time="57600" value="1.8$/kWh" />
  <item time="68400" value="1.3$/kWh" />
  <item time="75600" value="1.0$/kWh" />
</cprice>
<dprice repeat="8" period="86400">
  <item time="0" value="0.8$/kWh" />
  <item time="7200" value="0.5$/kWh" />
  <item time="21600" value="0.8$/kWh" />
  <item time="25200" value="1.1$/kWh" />
  <item time="32400" value="0.8$/kWh" />
  <item time="36000" value="0.5$/kWh" />
  <item time="38600" value="0.2$/kWh" />
  <item time="50400" value="0.5$/kWh" />
  <item time="54000" value="0.8$/kWh" />
  <item time="57600" value="1.6$/kWh" />
  <item time="68400" value="1.1$/kWh" />
  <item time="75600" value="0.8$/kWh" />
</dprice>
```

When policy = 1, a legal ESS looks like the following lines:
```xml
<ess ID="ESS10" Bus="B10" cap="3MWh" ec="0.9" ed="0.9" pc_max="1MW" pd_max="1MW" pf="0.95" init_elec="0.3MWh" policy="1">
  <ctime loop_period="86400" loop_times="8">
    <item btime="0" etime="21600" />
  </ctime>
  <dtime loop_period="86400" loop_times="8">
    <item btime="36000" etime="57600" />
  </dtime>
</ess>
```

## 7. Wind Turbine and Photovoltaic (PV) Panel
Wind turbines and PV panels share the same model. Element `<pv>` and `<wind>` are accepted. Following attributes should be provided.
+ ID: name of the device
+ Bus: to which bus the device connects
+ pf: power factor
+ cc: curtailment cost of unit power output. Due to various reasons, the output of PV panels and wind turbines may be curtailed. This coefficient is to describe this phenomenon.

PV example:
```xml
<pv ID="PV9" Bus="B9" pf="0.92" cc="1.0$/kWh">
  <P repeat="8" period="86400">
    <item time="0" value="0MW" />
    <item time="3600" value="0MW" />
    <item time="7200" value="0MW" />
    <item time="10800" value="0MW" />
    <item time="14400" value="0MW" />
    <item time="18000" value="0MW" />
    <item time="21600" value="0.0435MW" />
    <item time="25200" value="0.1540MW" />
    <item time="28800" value="0.2690MW" />
    <item time="32400" value="0.3325MW" />
    <item time="36000" value="0.4015MW" />
    <item time="39600" value="0.4720MW" />
    <item time="43200" value="0.5050MW" />
    <item time="46800" value="0.5802MW" />
    <item time="50400" value="0.5700MW" />
    <item time="54000" value="0.4235MW" />
    <item time="57600" value="0.3185MW" />
    <item time="61200" value="0.1980MW" />
    <item time="64800" value="0.0455MW" />
    <item time="68400" value="0MW" />
    <item time="72000" value="0MW" />
    <item time="75600" value="0MW" />
    <item time="79200" value="0MW" />
    <item time="82800" value="0MW" />
  </P>
</pv>
```

Wind turbine example:
```xml
<wind ID="W12" Bus="B12" MinV="0.9" MaxV="1.1" pf="0.92" cc="0.9$/kWh">
  <P repeat="8" period="86400">
    <item time="0" value="0.4220MW" />
    <item time="3600" value="0.1420MW" />
    <item time="7200" value="0.3020MW" />
    <item time="10800" value="0.2426MW" />
    <item time="14400" value="0.1024MW" />
    <item time="18000" value="0.4340MW" />
    <item time="21600" value="0.0435MW" />
    <item time="25200" value="0.2540MW" />
    <item time="28800" value="0.4690MW" />
    <item time="32400" value="0.3325MW" />
    <item time="36000" value="0.4015MW" />
    <item time="39600" value="0.4720MW" />
    <item time="43200" value="0.5050MW" />
    <item time="46800" value="0.1802MW" />
    <item time="50400" value="0.3700MW" />
    <item time="54000" value="0.4235MW" />
    <item time="57600" value="0.5185MW" />
    <item time="61200" value="0.1980MW" />
    <item time="64800" value="0.3455MW" />
    <item time="68400" value="0.2839MW" />
    <item time="72000" value="0.3280MW" />
    <item time="75600" value="0.2789MW" />
    <item time="79200" value="0.3253MW" />
    <item time="82800" value="0.2563MW" />
  </P>
</pv>
```

## 8. Grid template
It is easy to quickly create distribution networks based on IEEE33 node case and IEEE69 node case. You only need to add the 'model' attribute on the `<grid>` root element to import buses, lines, and generators from the template with just one line.

Please note that importing into the power grid defaults to a fixed load. You need to set the `fixed-load` property to `false` to make it a segmented function load, and set the `load-repeat` property to specify the number of repetitions (changes and periods cannot be manually specified for now). `The `grid-repeat` attribute is used for template repetition multiple times, such as creating 10 IEEE33 distribution networks (10 islands).

Here is a legal example:
```xml
<grid Sb="1MVA" Ub="10kV" model="ieee33" fixed-load ="false" grid-repeat="2" load-repeat="8" />
```