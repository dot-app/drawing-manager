# 简介

引入turf修正百度地图BMapGLLib计算面积不准确的bug；  
进行一些简单的扩展；  
并支持VUE使用；  
增加对手机端支持；


# HTML 环境使用

## 引入文件
```HTML
<!-- 引入文件 -->
<link href="//cdn.jsdelivr.net/gh/dot-app/drawing-manager/index.min.css" rel="stylesheet" />
<script type="text/javascript" src="//cdn.jsdelivr.net/gh/dot-app/drawing-manager/index.min.js"></script>
 
```

## 用法

```JS
var styleOptions = {
    strokeColor: "#5E87DB", // 边线颜色
    fillColor: "#5E87DB", // 填充颜色。当参数为空时，圆形没有填充颜色
    strokeWeight: 2, // 边线宽度，以像素为单位
    strokeOpacity: 1, // 边线透明度，取值范围0-1
    fillOpacity: 0.2, // 填充透明度，取值范围0-1
};
var labelOptions = {
    borderRadius: "2px",
    background: "#FFFBCC",
    border: "1px solid #E1E1E1",
    color: "#703A04",
    fontSize: "12px",
    letterSpacing: "0",
    padding: "5px",
};

// 实例化鼠标绘制工具
var drawingManager = new BMapGLLib.DrawingManager(map, {
    // isOpen: true,        // 是否开启绘制模式
    enableCalculate: true, // 绘制是否进行测距测面
    enableSorption: true, // 是否开启边界吸附功能
    sorptiondistance: 20, // 边界吸附距离
    circleOptions: styleOptions, // 圆的样式
    polylineOptions: styleOptions, // 线的样式
    polygonOptions: styleOptions, // 多边形的样式
    rectangleOptions: styleOptions, // 矩形的样式
    labelOptions: labelOptions, // label样式
    
    enablePrintResult: true, // 显示计算的长度/面积
    enableRemove: true, //是否允许删除已经标注的图层
});


// 绘制完成后获取相关的信息(面积等)
drawingManager.addEventListener("overlaycomplete", function (e) {
    console.log(e);
});
```
# VUE 环境使用
``` js
const BMAP_DRAWING_MARKER = "marker";
const BMAP_DRAWING_POLYLINE = "polyline";
const BMAP_DRAWING_CIRCLE = "circle";
const BMAP_DRAWING_RECTANGLE = "rectangle";
const BMAP_DRAWING_POLYGON = "polygon";

const loadJS = (url: string) => {
	return new Promise(function (resolve, reject) {
		window.onload = function () {
			resolve(true);
		};
		var script = document.createElement("script");
		script.type = "text/javascript";
		script.src = url;
		script.onerror = reject;
		script.onload = resolve;
		document.head.appendChild(script);
	});
};
const dynamicLoadCss = (url: string) => {
    var link = document.createElement("link");
    link.type = "text/css";
    link.rel = "stylesheet";
    link.href = url;
    document.head.appendChild(link);
};

let DrawingManager;
const draw = async (e) => {
    let $parent = getParent(proxy.$parent);
    map = $parent.map;
    BMap = $parent.BMap;
    console.log(BMap);

    if (!DrawingManager) {
        dynamicLoadCss('http://10.10.10.10:5501/DrawingManager.css');
        await loadJS('http://10.10.10.10:5501/DrawingManager.js');
        DrawingManager = window['BMapGLLib'].DrawingManager;
    }

    var styleOptions = {
        strokeColor: "#5E87DB", // 边线颜色
        fillColor: "#5E87DB", // 填充颜色。当参数为空时，圆形没有填充颜色
        strokeWeight: 2, // 边线宽度，以像素为单位
        strokeOpacity: 1, // 边线透明度，取值范围0-1
        fillOpacity: 0.2, // 填充透明度，取值范围0-1
    };
    var labelOptions = {
        borderRadius: "2px",
        background: "#FFFBCC",
        border: "1px solid #E1E1E1",
        color: "#703A04",
        fontSize: "12px",
        letterSpacing: "0",
        padding: "5px",
    };

    // 实例化鼠标绘制工具
    let drawingManager = new DrawingManager(map, {
        // isOpen: true,        // 是否开启绘制模式
        enableCalculate: true, // 绘制是否进行测距测面
        enableSorption: true, // 是否开启边界吸附功能
        sorptiondistance: 20, // 边界吸附距离
        circleOptions: styleOptions, // 圆的样式
        polylineOptions: styleOptions, // 线的样式
        polygonOptions: styleOptions, // 多边形的样式
        rectangleOptions: styleOptions, // 矩形的样式
        labelOptions: labelOptions, // label样式

        enablePrintResult: true, // 显示计算的长度/面积
        enableRemove: true, //是否允许删除已经标注的图层
    });


    drawingType = e.type;
    console.log(drawingManager);

    // 进行绘制
    if (drawingManager._isOpen && drawingManager.getDrawingMode() === drawingType) {
        drawingManager.close();
    } else {
        drawingManager.setDrawingMode(drawingType);
        drawingManager.open();
    }

    // 绘制完成后获取相关的信息(面积等)
    drawingManager.addEventListener("overlaycomplete", function (e) {
        //alert(e.calculate);
        drawingType = "";
        console.log(e.calculate);
    });
}

```

# 更新
DrawingManager 实例化参数增加：

```
enablePrintResult: true,    // 显示计算的长度/面积
enableRemove: true,         //是否允许删除已经标注的图层
```


# 相关链接

[BMapGLLib](https://github.com/huiyan-fe/BMapGLLib)  
[turf](https://github.com/Turfjs/turf)

