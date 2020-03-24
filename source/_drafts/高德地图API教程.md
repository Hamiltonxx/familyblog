---
title: 高德地图API教程
tags:
---
# 前言
到目前为止，WEB高德地图提供了JSAPI v1.4.15和v2.0 Beta6两个版本。

# 准备
```
<script type="text/javascript" src="https://webapi.amap.com/maps?v=1.4.15&key=9aad105e3d31abe9233b566bb28a1ccf"></script>

<div id="container"></div>

#container {width:1024px; height:768px;}
```

# 动手
```
// const map = new AMap.Map('container');
const map = new AMap.Map('container', {
    zoom: 13,
    center: [121.47, 31.23]
});
```

## 图层
默认情况下，地图只显示标准底图，如需要叠加别的图层，可以通过map.add方法添加图层
```
//实时路况图层
const trafficLayer = new AMap.TileLayer.Traffic({
    zIndex: 10
});
map.add(trafficLayer);
```
也可以在地图初始化时通过layers属性为地图设置多个图层
```
const map = AMap.Map('container', {
    center: [121.47, 31.23],
    zoom: 13,
    zooms: [4,18], //地图级别范围
    layers: [
        new AMap.TileLayer.Satellite(),
        new AMap.TileLayer.RoadNet()
    ]
});
```

## 标记与矢量图形
```
const marker = new AMap.Marker({
    position: [121.47, 31.23]
})
map.add(marker);
```