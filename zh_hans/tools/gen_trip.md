# 车辆与行程生成器

命令：
```
python gen_trip.py \
    -d <项目文件夹> \
    -n <车辆数量> \
    [-c <行程参数文件夹>] \
    [-v <车对网参与概率>] \
    [--day <行程天数>] \
    [--mode <auto|taz|poly>] \
    [--cache-route <none|runtime|static>] \
    [--seed <随机种子>] \
```

+ **d**: 项目文件夹。例如：`cases/std_12nodes`
+ **n**: 要生成的车辆数量。例如：`10000`
+ **c**: 行程参数文件夹。默认：`v2sim/probtable`。
+ **v**: 每个用户愿意参与车对网的概率。默认：`1.0`。
+ **day**: 行程生成的天数。默认：`7`。***注意***：将会额外生成一天。请参阅 [genveh 页面](zh_hans/usage/2_4_genveh) 了解详情。
+ **mode**: 行程生成模式。共有 3 种模式，在 [genveh 页面](zh_hans/usage/2_4_genveh) 中有详细描述。默认：`auto`。
+ **cache-route**: 路由缓存的存储方式，在 [genveh 页面](zh_hans/usage/2_4_genveh) 中有描述。默认：`none`。
+ **seed**: 随机种子。默认：当前时间的纳秒值。