---
title: openLayer的使用
layout: page
category: openLayer
date: 2014-06-06
---
先看效果
```
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script type="text/javascript" src="http://www.openlayers.cn/olapi/OpenLayers.js"></script>
	<style>
	  html,body{width:100%;height:100%;margin:0;padding:0;}
	</style>
	<script>
	  function init(){
	  	var map = new OpenLayers.Map('rcp1_map');
	  	var osm = new OpenLayers.Layer.OSM();
	  	map.addLayer(osm);
	  	map.zoomToMaxExtent();
	  }
	</script>
</head>
<body onload="init()">
<div id="rcp1_map" style="width:100%;height:100%;"></div>  	
</body>
</html>
```
