# 创建案例
本节将介绍如何使用 OSM Web Wizard 创建路网。

## 简介
OSM Web Wizard 是 SUMO 提供的一个工具，用于从 OpenStreetMap 捕获路网。它并非专为 V2Sim 设计。

**注意**：由于网络限制，**中国大陆的用户**可能无法访问 OpenStreetMap。

## 打开 OSM Web Wizard
OSM Web Wizard 通常位于 SUMO 的安装文件夹中。

![osm_place](https://github.com/user-attachments/assets/b7854f3a-f087-4ea1-b7c5-38b4d643e738)

您也可以在"开始"菜单中直接搜索并打开它。

## 定位目标区域
您可以输入城市名称或使用经纬度来定位目标区域。默认情况下，它会下载您看到的整个区域。如果您只需要部分区域，请勾选 `选择区域` 并通过拖动选框边界来选择您想要的区域。

![osm1](https://github.com/user-attachments/assets/bdb1fdf5-529c-4813-b7b6-1e1531828425)

在"选项"列中，请勾选"添加多边形"，因为它们将用于电动汽车充电站和行程生成。

## 清除行程
取消选中所有复选框，以防止在此处生成任何行程。行程生成应在 V2Sim 中完成，因为其遵循的协议与 SUMO 不同。

![osm2](https://github.com/user-attachments/assets/9af8d30e-620d-4b87-a1f8-382a911d4175)

## 选择合适的边
只应保留"highway"（高速公路/主要道路）类型的边。保留其他类型的边是不必要的，并且可能会降低性能。

![osm3](https://github.com/user-attachments/assets/cefa9d20-893f-4b8e-b9c8-68400860d618)

## 生成案例
点击绿色的"Generate Scenario"按钮来创建您的案例。之后，将您使用 OSM Web Wizard 生成的案例复制到合适的位置，例如您下载的源代码中的 `case` 文件夹。然后通过以下命令在命令提示符中运行 `gui_main.py`：
```bash
python gui_main.py
```

点击 `Add project...` 选中并使用`Open`打开您创建的文件夹。