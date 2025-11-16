# 高级绘图工具

本程序是一个用于高级图形绘制的命令行工具。

### 命令
+ `plot <series_name> [<label> <color> <linestyle> <side>]`: 向图中添加一个数据系列
+ `title <title>`: 设置图的标题
+ `xlabel <label>`: 设置 X 轴标签
+ `yleftlabel/ylabel <label>`: 设置左侧 Y 轴标签
+ `yrightlabel <label>`: 设置右侧 Y 轴标签
+ `yticks <ticks> [<labels>]`: 设置 Y 轴刻度和标签
+ `legend <loc>`: 设置图例位置
+ `save <path>`: 将图保存到指定路径并关闭图形
+ `exit`: 退出绘图

系列名称的格式应为 `<results>:<attribute>:<instances>:<start_time>`，其中：
+ "results" 是结果文件夹，
+ "attribute" 是要绘制的属性，可以是：

| 充电站 | 电动汽车 & 光伏/风电 | 发电机 & 线路 | 母线 & 储能系统 |
|---|---|---|---|
| cs_load | ev_soc | gen_active | bus_active_gen |
| cs_wait_count | ev_cost | gen_reactive | bus_reactive_gen |
| cs_net_load | ev_status | gen_costp | bus_active_load |
| cs_price_buy | ev_cpure | line_active | bus_reactive_load |
| cs_price_sell | ev_earn | line_reactive | bus_shadow_price |
| cs_discharge_load | pvw_p | line_current | ess_p |
| cs_v2g_cap | pvw_cr | | ess_soc |

+ "instances" 是实例名称，例如：
    "CS1", "CS2", `<all>`, `<fast>`, `<slow>` 用于充电站，
    "v1", "v2" 用于电动汽车，
    "G1", "G2" 用于发电机，
    "B1", "B2" 用于母线，
    等等。
+ "start_time" 是系列的开始时间。默认为 "0"。此为可选参数。

### 示例
```
plot "results:cs_load:CS1" "CS1 负荷" "blue" "-" "left"
plot "results:cs_load:CS2" "CS2 负荷" "red" "--" "left"
title "CS1 和 CS2 负荷"
xlabel "时间"
yleftlabel "负荷/千瓦时"
legend
save "test.png"
```

您可以在命令提示符/终端中将命令文件作为参数加载。