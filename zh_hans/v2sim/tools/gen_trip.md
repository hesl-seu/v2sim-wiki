# 车辆与行程生成器

命令:
```
python gen_trip.py \
    -d <project folder> \
    -n <number of vehicles> \
    [-c <trip parameter folder>] \
    [-v <V2G probability>] \
    [--day <number of trip days>] \
    [--mode <auto|taz|poly>] \
    [--cache-route <none|runtime|static>] \
    [--seed <randomization seed>] \
```

+ **d**: 项目文件夹。例如：`cases/std_12nodes`
+ **n**: 要生成的车辆数。例如：`10000`
+ **c**: 行程参数文件夹。 默认值：`v2sim/probtable`
+ **v**: 每辆车参与V2G的概率。默认值：`1.0`
+ **day**: 生成行程的天数。 默认值: `7`. **注意**: 会额外生成一天的行程。请阅读[genveh page](genveh)以查看细节。
+ **mode**: 如何生成行程。包含3种模式。请阅读[genveh page](genveh)以查看细节。默认值：`auto`。
+ **cache-route**: 如何设置行程缓存。请阅读[genveh page](genveh)以查看细节。默认值：`none`。
+ **seed**: 随机化种子。默认值：当前时间（纳秒）。