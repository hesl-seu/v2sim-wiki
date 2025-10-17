# 充电站生成程序

命令：
```
python gen_cs.py \ 
    -d <SUMO configuration folder> \
    [--type <scs|fcs>] \
    [--slots <number of charging piles>] \
    [--pbuy <purchase price>] \
    [--randomize-pbuy] \
    [--psell <selling price>] \
    [--randomize-psell] \
    [--seed <randomization seed>] \
    [--n-cs <number of charging stations>] \
    [--cs-names <CS names>] \
    [--n-bus <number of buses>] \
    [--bus-names <buses used>] \
```

+ d: 项目文件夹
+ type: 充电站类型, 默认fcs, 可选fcs(快充站), scs(慢充站)
+ slots: 充电桩数量, 默认10
+ pbuy: 购电价格, 默认1.5元/kWh
+ randomize-pbuy: 随机化购电价格
+ psell: 售电价格, 默认1元/kWh
+ randomize-psell: 随机化售电价格
+ seed: 随机化种子, 不指定则使用当前时间(纳秒)

+ 充电站选择:
  - 默认为使用所有可用的充电站, 可以通过以下选项改变:
  - n-cs: 快充站数量, 指定该项则快充站选择方式为"随机"
  - cs-names: 快充站名称, 指定该项则快充站选择方式为"指定"
  - 以上两项不能一起使用

+ 母线选择:
  - 默认使用所有母线, 可以通过以下选项改变:
  - n-bus: 使用的母线数量, 指定该项则母线选择方式为"随机"
  - bus-names: 使用的母线, 指定该项则母线选择方式为"指定"
  - 以上两项不能一起使用