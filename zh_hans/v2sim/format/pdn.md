# XML格式配电网文件
XML格式的基本格式如下：
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

## 1. grid根节点
配电网XML文件开始于`<grid>`根节点，它必须包含两个属性`Sb`和`Ub`，分别表示基准功率和基准电压。值必须包含单位：对于`Sb`，支持`MVA`和`kVA`两种单位；对于`Ub`，仅支持`kV`这一种单位。注意：单位的拼写区分大小写。

以下是2个合法的例子：
```xml
<grid Sb="1MVA" Ub="10.0kV">...</grid>
<grid Sb="500.0kVA" Ub="10.0kV">...</grid>
```

**如果您想快速创建IEEE33和IEEE69配电网，请跳到第6节。**

## 2. 母线：bus节点
我们用`<bus>`节点表示一个母线，而母线名称、母线负荷则是必要的参数。母线名称是bus节点的`ID`属性所指定的，而母线负荷则是由下属的`<Pd>`和`<Qd>`两个子节点所定义的。

### 2.1 固定负荷
母线负荷可以是固定的，也可以随时间变化。对于前一种情况，例如负荷恒定为2.5+j1.0MVA，则应当表示如下：
```xml
<bus ID="bus_name">
  <Pd const="2.5MW" />
  <Qd const="1.0Mvar" />
</bus>
```
恒定负荷用`const`属性表示，单位支持`MW`，`kW`(有功)，`Mvar`，`kvar`(无功)和`pu`，这里`pu`表示标幺值(per unit value)。在这种情况下，Pd和Qd节点没有子节点或文本，因此应当立即关闭。

另一种可以接受的写法如下所示：
```xml
<bus ID="bus_name">
  <Pd const="2.5" unit="MW" />
  <Qd const="1.0" unit="Mvar" />
</bus>
```
在这种情况下，值和单位被分开记录。

### 2.2 分段函数负荷
当负荷随着时间变化时，例如，从0s开始为2.5+j1.0MVA，而在3600s开始变为3.0+j1.2MVA，则表示如下：
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

由上可知，分段函数式的负荷可用`<item>`子节点表示，其`time`属性表示值的开始时间，`value`属性表示所取的值，单位同上。
多个`<item>`子节点表示多个分段。

为了表示周期性发生的负荷，可以在`<Pd>`和`<Qd>`节点上添加`repeat`和`period`参数，表示负荷重复的次数和周期，例如：

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

上述内容等价于：

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

请注意，周期性负荷不支持将值和单位分开记录。

## 3. 线路：line节点
我们用`<line>`节点表示一条线路。对于一条线路，需要提供其名称（属性`ID`）、起始母线（属性`From`）、中止母线（属性`To`）、电阻（属性`R`）和电抗（属性`X`），阻抗参数的单位总是欧姆，**不需要**填写在XML文件中。

以下是一个合法的例子
```xml
<line ID="line1" From="bus1" To="bus2" R="0.04ohm" X="0.002ohm" />
```

## 4. 发电机：gen节点
我们用`<gen>`节点表示一个发电机（拼写成`generator`也可以接受）。对于一个发电机，需要提供其名称和所在母线，分别对应属性`ID`和`Bus`。发电功率的上下限(对应子节点`Pmin`,`Pmax`,`Qmin`,`Qmax`)需要被提供，其形式应当与母线的`Pd`和`Qd`项目一致。发电成本系数(固定成本、一次成本、二次成本)也需要提供，分别记为子节点`<CostA>`,`<CostB>`和`<CostC>`。

成本系数同样提供固定值和分段函数2种表示形式，类似于Pd和Qd。但是，成本系数的单位是不一样的。对于二次成本，支持的单位有`$/kWh2`,`$/MWh2`和`$/puh2`；对于一次成本，支持的单位有`$/kWh`,`$/MWh`和`$/puh`；对于固定成本，支持的单位为`$`，由于只有一种可用单位，因此可以省略。

以下是一个合法的例子：
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

## 5. 发电机模型：genmodel节点
考虑到配电网种可能存在多个同型号发电机，因此特设置发电机模型`<genmodel>`节点，以减少输入量。拼写`<generatormodel>`也可以接受。发电机模型除了不需要`ID`和`Bus`属性，其他与发电机节点一致。此外，`name`属性需要被提供，以表示模型名称。在使用发电机模型时，须添加`model`属性以指定所用的模型。

以下是一个合法的例子：
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

它等价于：
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

## 6. 储能元件
按照电网价格指定充放电策略(policy=2)：
```xml
<ess ID="ESS10" Bus="B10" cap="1MWh" ec="0.9" ed="0.9" 
    pc_max="100kW" pd_max="100kW" pf="0.95" init_elec="0.1MWh"
    policy="2" cprice="0.8$/kWh" dprice="1.2$/kWh" />
```
cprice表示严格低于此价格充电，dprice表示严格高于此价格放电。
此时应当在`<grid>`根节点添加价格元素，例如：
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

或者也可以指定充放电时间(policy=1)：
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

## 7. 风电和光伏
风电和光伏共用一种模型，需要提供如下属性：
+ ID: 设备名称
+ Bus: 连接到的母线
+ pf: 功率因数
+ cc: 单位弃风/弃光成本

光伏例子：
```xml
<pv ID="PV9" Bus="B9" MinV="0.9" MaxV="1.1" pf="0.92" cc="1.0$/kWh">
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
风电例子：
```xml
<pv ID="W12" Bus="B12" MinV="0.9" MaxV="1.1" pf="0.92" cc="0.9$/kWh">
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

## 8. 配电网模板
对于XML格式的配电网描述文件，快速创建基于IEEE33节点和IEEE69节点配电网是容易的。您只需要在`<grid>`根节点上添加`model`属性，即可从模板中一键导入母线、线路和发电机。

请注意，导入电网默认是固定负荷，您需要设置`fixed-load`属性为`false`使其变为分段函数负荷，设置`load-repeat`属性以指定重复次数(变化情况和周期暂时不能手动指定)。`grid-repeat`属性则用于模板多次重复，例如创建10个IEEE33配电网（10个孤岛）。

以下是一个合法的例子：
```xml
<grid Sb="1MVA" Ub="10kV" model="ieee33" fixed-load ="false" grid-repeat="2" load-repeat="8" />
```