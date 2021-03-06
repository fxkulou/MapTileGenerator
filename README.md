# MapTileGenerator
支持TMS、WMTS标准瓦片下载，支持百度地图瓦片、高德地图瓦片、腾讯地图瓦片、天地图、ArcGIS Rest、geoserver等瓦片下载。默认以png文件方式保存瓦片，也支持以sqlite（[mbtiles格式](https://github.com/mapbox/mbtiles-spec)）保存瓦片。

## 使用说明：
设置mapConfig.json，根据配置项请求瓦片，支持多线程方式。下载失败的瓦片用sqlite数据库保存在{savePath}\fails.db，下次启动程序时会重新下载失败瓦片。程序运行中途退出时，下次启动程序将会从上次退出的进度继续下载。

### geoserver wms瓦片下载配置  
    {        
        "resolutions" :   [128, 64,32],  
        "tileSize" : [256,256],  
        "extent" : [12662882.864568064,2543518.577818389,12761187.939702341,2599303.8005401082],
        "origin": [0,0],
        "offsetZoom" :11,//resolutions如果设置了某一部分级别，必须要设置这个偏移量;
        "type" : "wms",
        "url": "http://localhost:8080/geoserver/szgas/wms",	
        "urlParas" : {
          "FORMAT" : "image/png",
          "VERSION" : "1.1.1",
          "STYLES" : "",
          "LAYERS" : "szgas:jd_baidu",
          "REQUEST" : "GetMap",
          "SRS" : "EPSG:3857",
          "TRANSPARENT" : true
        },
        "runThreadCount" : 5,
        "savePath" : "" //不设置保存路径，程序根目录是默认的瓦片保存路径；
    }


### 天地图WMTS瓦片下载配置

    {
      "resolutions" :   [ 0.703125,  0.3515625, 0.17578125],
      "tileSize" : [256,256],
      "extent" : [-180.0 -90.0,180.0,90.0],
      "origin": [-180,90],
      "offsetZoom" :1,//resolutions如果设置了某一部分级别，必须要设置这个偏移量;
      "type" : "wmts",
      "url": "http://172.16.12.15:8080/dfc/services/ogc/wmts/vec",
      "urlParas" : {
        "SERVICE" : "WMTS",
        "FORMAT" : "image/png",
        "VERSION" : "1.0.0",
        "STYLES" : "default",
        "LAYER" : "vec",
        "REQUEST" : "GetTile",
        "TileMatrixSet" : "CustomCRS4326Scalevec",
        "TRANSPARENT" : true
      },
      "runThreadCount" : 5,
      "savePath" : ""
    }


### 天地图WMS下载

    {
      "resolutions": [ 1.40625, 0.703125, 0.3515625 ],
      "tileSize": [ 256, 256 ],
      "extent": [ -180, -90, 180, 90 ],
      "origin": [ -180, -90 ],
      "offsetZoom": 1,//resolutions如果设置了某一部分级别，必须要设置这个偏移量;
      "type": "wms",
      "url": "http://www.scgis.net.cn/iMap/iMapServer/defaultRest/services/sctilemap/WMS",
      "urlParas": {
        "FORMAT": "image/png",
        "VERSION": "1.1.1",
        "STYLES": "",
        "LAYERS": "0",
        "REQUEST": "GetMap",
        "SRS": "EPSG:4326",
        "TRANSPARENT": true
      },
      "runThreadCount": 5,
      "savePath": ""
    }

### ArcServerRest瓦片下载

    {
      "resolutions": [
        0.0013732916427489112,
        0.0006866458213744556
      ],
      "tileSize": [ 256, 256 ],
      "extent": [ 107.86896617100007, 30.390792641000075, 108.90726196600006, 31.005204326000076 ],
      "origin": [ -400.0, 399.9999999999998 ],
      "offsetZoom": 0,//resolutions如果设置了某一部分级别，必须要设置这个偏移量;
      "type": "ArcServerRest",
      "url": "http://222.180.68.94:6080/arcgis/rest/services/wzpsp/wzmap/MapServer/tile/{z}/{y}/{x}",
      "runThreadCount": 1,
      "savePath": ""
    } 

 
 ### ArcServer本地缓存的瓦片下载，目的是为了生成标准的WMTS xyz方式的瓦片或sqlite存储的瓦片
    {
    "resolutions": [   
              0.00015228550437313792,
              0.000076142752186568962,
              0.000038071376093284481,
              0.00001903568804664224,
              0.0000095178440233211202
              //,0.0000047589220116605601,
              //0.0000023794610058302801
    ],
    "tileSize": [ 256, 256 ],
    "extent": [ 113.3946038380694, 34.0858360155829, 113.5505441945475, 34.2026389974371 ],
    "origin": [ -400.0, 400 ],
    "offsetZoom": 3, //resolutions如果设置了某一部分级别，必须要设置这个偏移量;
    "type": "ArcServerLocalTile",
    "output": "sqlite", //用sqlite保存瓦片；默认是文件方式存储瓦片
    "url": "http://192.168.20.34/YZMAP/{z}/{y}/{x}.png",
    "runThreadCount": 1,
    "savePath": ""
  }

### 百度地图下载

    {
      //"resolutions": [ 128, 64, 32 ],
      //"resolutions": [ 262144, 131072, 65536, 32768, 16384, 8192, 4096, 2048, 1024, 512, 256, 128, 64, 32, 16, 8, 4, 2, 1 ],//完整的resolutions
      "resolutions": [  256,128,64,32,16 ],
      "offsetZoom": 10, //resolutions如果设置了某一部分级别，必须要设置这个偏移量;
      "tileSize": [ 256,256 ],
      "extent": [ 11808770.385317, 3403500.2612752, 11892674.468269, 3442476.25119 ],
      "origin": [ 0, 0 ],
      "type": "baidu",
      "output": "sqlite",//用sqlite保存瓦片；默认是png文件方式存储瓦片
       "url": "http://online3.map.bdimg.com/onlinelabel/?qt=tile&styles=pl&udt=20151021&scaler=1&p=1&qt=tile&x={x}&y={y}&z={z}",
      "runThreadCount":5
    }

  ### 腾讯地图下载 
    {
      "resolutions": [  
          //156543.0339,  
          //78271.516953125,  
          //39135.7584765625,  
          //19567.87923828125,  
          9783.939619140625  
          ,4891.9698095703125  //测试需要,只下载2级；
          //,2445.9849047851562  
          //,1222.9924523925781  
          //,611.4962261962891,  
          //305.74811309814453,  
          //152.87405654907226,  
          //76.43702827453613,  
          //38.218514137268066,  
          //19.109257068634033,  
          //9.554628534317016,  
          //4.777314267158508, 
          //2.388657133579254,
          //1.194328566789627,
          //0.5971642833948135
      ],  
      "offsetZoom": 4, //resolutions如果设置了某一部分级别，必须要设置这个偏移量;
      "tileSize": [ 256, 256 ],
      "extent": [-20037508.342789244, -20037508.342789244, 20037508.342789244, 20037508.342789244],
      "origin": [-20037508.342789244, -20037508.342789244],
      "type": "tencent",
      "url": "http://rt1.map.gtimg.com/realtimerender?z={z}&x={x}&y={y}&type=vector&style=0&v=1.1.2",
      "runThreadCount": 1
    }

### 高德地图下载 
    {
      "resolutions": [  
          //156543.0339,  
          //78271.516953125,  
          //39135.7584765625,  
          19567.87923828125,  
          9783.939619140625  
          //,4891.9698095703125  //测试需要,只下载2级；
          //,2445.9849047851562  
          //,1222.9924523925781  
          //,611.4962261962891,  
          //305.74811309814453,  
          //152.87405654907226,  
          //76.43702827453613,  
          //38.218514137268066,  
          //19.109257068634033,  
          //9.554628534317016,  
          //4.777314267158508, 
          //2.388657133579254,
          //1.194328566789627,
          //0.5971642833948135
      ],  
      "offsetZoom": 3, //resolutions如果设置了某一部分级别，必须要设置这个偏移量;
      "tileSize": [ 256, 256 ],
      "extent": [-20037508.342789244, -20037508.342789244, 20037508.342789244, 20037508.342789244],
      "origin": [-20037508.342789244, 20037508.342789244],
      "type": "gaode",
      "url": "http://webrd01.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}",
      "runThreadCount": 1
    }


## 瓦片规则

瓦片存储路径：{savePath}\Tiles\Zoom+offsetZoom\x\y.png
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2137628-204d853cce816a7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2137628-77c29b8d13114922.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
