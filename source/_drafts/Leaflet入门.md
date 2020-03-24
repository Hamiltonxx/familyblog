---
title: Leaflet入门
tags:
---
# 简介
Leaflet是专为移动和交互而生的最优秀的开源Javascript库。大小只有38KB,它有地图开发者所需的绝大多数特征。
Leaflet秉持着简单、性能、可用的理念，在所有desktop和mobile平台都能高效工作，有很多plugin以及美观、易用和文档丰富的API。
下面的示例，在'map' div上创建map,增加tile和增加marker，简单易懂。
```
const map = L.map('map').setView([51.505, -0.09], 13);

L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
}).addTo(map);

L.marker([51.5, -0.09]).addTo(map)
    .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')
    .openPopup();
```

# 走你
## 准备工作
```
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.6.0/dist/leaflet.css"
   integrity="sha512-xwE/Az9zrjBIphAcBb3F6JVqxf46+CDLwfLMHloNu6KEQCAWi6HcDUbeOfBIptF7tcCzusKFjFw2yuvEpDL9wQ=="
   crossorigin=""/>

<!-- Make sure you put this AFTER Leaflet's CSS -->
 <script src="https://unpkg.com/leaflet@1.6.0/dist/leaflet.js"
   integrity="sha512-gZwIG9x3wUXg2hdXF6+rVkLF/0Vi9U8D2Ntg4Ga5I5BZpVkVxlJWbSQtXPSiUTtC0TjtGOmxa1AJPuV0CPthew=="
   crossorigin=""></script>

<div id="mapid"></div>

#mapid { height: 180px; }
```
## 创建地图
让我们以北纬31度23、东经121度47的上海人民广场(121.47004,  31.23136)为中心点，用漂亮的Mapbox Streets tiles创建地图。
```
const mymap = L.map('mapid').setView([121.47, 31.23], 13);
```
接下来，增加一个tile layer,我们用Mapbox Streets tile layer.