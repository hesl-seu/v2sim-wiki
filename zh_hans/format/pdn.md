# 配电网文件格式

配电网文件遵循 XML 格式。一个配电网文件如下所示：

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

## 1. 根元素：`grid`
配电网文件以 `<grid>` 根元素开始。它必须包含两个属性：`Sb` 和 `Ub`，分别代表基准功率和基准电压。属性必须包含单位。对于 `Sb`，支持 `MVA` 和 `kVA`；对于 `Ub`，仅支持 `kV`。请注意，单位区分大小写。

以下是 2 个合法的示例：
```xml
<grid Sb="1MVA" Ub="10.0kV">...</grid>
<grid Sb="500.0kVA" Ub="10.0kV">...</grid>
```

**如果您想创建 IEEE33 或 IEEE69 网络，请转到第 6 节。**

## 2. 母线元素：`bus`
`<bus>` 元素表示一条母线，需要母线名称和母线负荷。母线名称通过属性 `ID` 指定，而母线负荷通过子元素 `<Pd>` 和 `<Qd>` 指定。

### 2.1 固定负荷
母线的负荷可以是固定的或可变的。固定负荷，例如 2.5+j1.0 MVA，可以通过以下行描述：
```xml
<bus ID="bus_name">
  <Pd const="2.5MW" />
  <Qd const="1.0Mvar" />
</bus>
```
固定负荷由属性 `const` 表示，支持的单位有 `MW`、`kW`（有功功率）、`Mvar`、`kvar`（无功功率）和 `pu`。这里 `pu` 表示标幺值。在固定负荷情况下，禁止为 Pd 和 Qd 元素使用子元素。

固定负荷的另一个合法示例：
```xml
<bus ID="bus_name">
  <Pd const="2.5" unit="MW" />
  <Qd const="1.0" unit="Mvar" />
</bus>
```
其中值和其单位是分开的。

### 2.2 分段函数负荷
当母线的负荷随时间波动时，例如从 0 秒起为 2.5+j1.0 MVA，从 3600 秒起为 3.0+j1.2 MVA，应通过以下行描述：
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

分段函数负荷可以通过子元素 `<item>` 表示，其中属性 `time` 表示开始时间，属性 `value` 表示值。单位与固定负荷相同。多个 `<item>` 表示多个分段。

要表示周期性负荷，可以在 `<Pd>` 和 `<Qd>` 元素中添加属性 `repeat` 和 `period`。示例：

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

这等价于：

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

请注意，周期性负荷不支持分开的值和单位。

## 3. 线路元素：`line`
`<line>` 元素用于表示一条输电线路。对于一条线路，需要其名称（属性 `ID`）、源母线（属性 `From`）、终端母线（属性 `To`）、电阻（属性 `R`）和电抗（属性 `X`），其中 R 和 X 的单位始终为 **欧姆**。

以下是一个合法示例：
```xml
<line ID="line1" From="bus1" To="bus2" R="0.04ohm" X="0.002ohm" />
```

## 4. 发电机元素：`gen`
`<gen>` 或 `<generator>` 元素用于表示一台发电机，需要其名称（属性 `ID`）和母线（属性 `bus`）。还需要发电限制（属性 `Pmin`、`Pmax`、`Qmin`、`Qmax`），形式与母线的 `Pd` 或 `Qd` 属性相同。发电成本的系数（固定成本、一次成本、二次成本）也是必需的，在子元素 `<CostA>`、`<CostB>` 和 `<CostC>` 中表示。

成本系数也提供两种表示形式：固定值和分段函数，类似于 `Pd` 和 `Qd`。但是，成本系数的单位不同。对于二次成本，支持的单位是 `$/kWh2`、`$/MWh2` 和 `$/puh2`；对于一次成本，支持的单位是 `$/kWh`、`$/MWh` 和 `$/puh`；对于固定成本，支持的单位是 `$`，但由于只有一个可用单位，可以省略。

以下是一个合法示例：
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

## 5. 发电机模型元素：`genmodel`
考虑到配电网中可能存在多个相同型号的发电机，设立了发电机模型元素 `<genmodel>`（或 `<generatormodel>`）以减少输入。发电机模型与发电机节点一致，只是不需要 'ID' 和 'bus' 属性。此外，需要提供 'name' 属性来表示模型名称。使用发电机模型时，必须添加 'model' 属性来指定所使用的模型。

以下是一个合法示例：
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

这等价于：
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

## 6. 储能系统元素：`ess`
一个储能系统如下所示：
```xml
<ess ID="ESS10" Bus="B10" cap="1MWh" ec="0.9" ed="0.9" 
    pc_max="100kW" pd_max="100kW" pf="0.95" init_elec="0.1MWh"
    policy="2" cprice="0.8$/kWh" dprice="1.2$/kWh" />
```
其中
+ ID: 储能系统的名称
+ Bus: 储能系统连接的母线
+ cap: 容量
+ ec: 充电效率，0~1
+ ed: 放电效率，0~1
+ pc_max: 最大充电功率
+ pd_max: 最大放电功率
+ pf: 功率因数
+ init_elec: 储能系统中存储的初始电量，0 < init_elec < cap
+ policy: 充放电策略
  - policy = 2: 按价格，由 `cprice` 和 `dprice` 控制
     - cprice: 当价格严格低于此价格时充电
     - dprice: 当价格严格高于此价格时放电
  - policy = 1: 按时间，由 `ctime` 和 `dtime` 控制
     - ctime: 在给定时间范围内充电
     - dtime: 在给定时间范围内放电

当 policy = 2 时，根元素中需要 `cprice` 和 `dprice` 元素。示例：
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

当 policy = 1 时，一个合法的储能系统如下所示：
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

## 7. 风力涡轮机和光伏面板
风力涡轮机和光伏面板共享相同的模型。接受元素 `<pv>` 和 `<wind>`。应提供以下属性。
+ ID: 设备名称
+ Bus: 设备连接的母线
+ pf: 功率因数
+ cc: 单位功率输出的削减成本。由于各种原因，光伏面板和风力涡轮机的输出可能会被削减。此系数用于描述这种现象。

光伏示例：
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

风力涡轮机示例：
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
</wind>
```

## 8. 电网模板
基于 IEEE33 节点案例和 IEEE69 节点案例快速创建配电网很容易。您只需要在 `<grid>` 根元素上添加 'model' 属性，即可用一行代码从模板导入母线、线路和发电机。

请注意，导入到电网默认为固定负荷。您需要将 `fixed-load` 属性设置为 `false` 以使其成为分段函数负荷，并设置 `load-repeat` 属性以指定重复次数（目前无法手动指定变化和周期）。`grid-repeat` 属性用于模板重复多次，例如创建 10 个 IEEE33 配电网（10 个孤岛）。

以下是一个合法示例：
```xml
<grid Sb="1MVA" Ub="10kV" model="ieee33" fixed-load ="false" grid-repeat="2" load-repeat="8" />
```