# EV充电负荷生成模拟器 - 使用帮助

命令：

```
python sim_single.py \
    -d <configuration folder> \
    [-b <start time>] \
    [-e <end time>] \
    [-l <data recording step>] \
    [-o <output folder>] \
    [--seed <random seed>] \
    [--log <data to be recorded>] \
    [--no-plg <plugins to be disabled>] \
    [--show] \
    [--no-daemon] \
    [--copy] \
    [--copy-state] \
    [--debug] \

python sim_single.py \
    [--file <file>]

python sim_single.py \
    [--ls-com] 
```

+ 以下参数应当单独使用, 不可与其他参数配合:
  - file: 从外部文件读取参数并串行的实行命令行仿真, 该文件的内容应当是一个以空格分隔的参数列表, 如果该文件有多行则只考虑第1行. 例如: "-d 12nodes -b 0 -e 172800"
  - ls-com: 列出所有可用的插件和记录项
+ 以下参数用于单次仿真:
  - d: 配置文件夹
  - b: 开始时间, 单位为秒, 默认从SUMO配置中读取
  - e: 结束时间, 单位为秒, 默认从SUMO配置中读取
  - l: 数据记录步长, 单位为秒, 默认值10
  - o: 输出文件夹, 默认为当前目录下的results文件夹
  - seed: 自动生成车辆、充电站以及SUMO使用的随机种子, 默认值为当前时间(ns)%65536
  - log: 要记录的数据, 包括电动汽车(ev), 快充站(fcs), 慢充站(scs), 发电机(gen), 母线(bus)和输电线(line), 多项数据用逗号隔开, 默认值为"fcs,scs"
  - no-plg: 禁用指定的插件, 多个插件用逗号隔开, 默认不禁用任何插件
  - copy: 仿真结束后将配置文件复制到输出文件夹, 默认不复制
  - gen-veh: 强制重新生成车辆文件的命令行, 默认不生成
  - gen-fcs: 强制重新生成充电站文件的命令行, 默认不生成
  - gen-scs: 强制重新生成充电站文件的命令行, 默认不生成
  - plot: 完成仿真后的绘图命令行, 默认不绘图
  - initial-state: 仿真的初始状态文件夹, 默认不加载(使用SUMO的默认行为)
  - load-last-state: 加载上次仿真保存的状态, 启用该项将使initial-state失效
  - save-on-abort: 当仿真意外中断时, 保存仿真状态
  - save-on-finish: 当仿真完成时, 保存仿真状态
  - route-algo: 寻路算法，默认为"CH"，可选项有"astar", "dijkstra", "CH"和"CHWrapper".
+ 以下参数用于图形化仿真:
  - show: 启用该选项以GUI模式启动. 该选项仅在Linux下有用, 在Windows下请调整v2sim/traffic/win_vis.py的WINDOWS_VISUALIZE来改变可见性级别
  - no-daemon: 启用该选项以将仿真线程与显示窗口分离, 不启用时显示窗口一旦关闭, 仿真也会停止
  - debug: 针对图形仿真启用调试模式, 图形仿真出错时会输出详细的错误信息