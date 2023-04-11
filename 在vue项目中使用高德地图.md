### 需求很重要，有需求你才有努力解决问题的方向，加油！

#### 在我们使用vue构建项目的时候，难免在业务需求上会遇到使用高德地图的时候，这时候问题就来了。

#### 我们该怎么在vue项目中插入高德地图？通过度娘我知道了vue-amap这个好东西，详细看官方文档：[vue-amap](https://elemefe.github.io/vue-amap/)

##### 安装vue-amap

1. 推荐npm安装

   ```javascript
   npm install vue-amap --save
   ```

2. 通过CDN引入

   ```javascript
   <script src="https://unpkg.com/vue-amap/dist/index.js"><script>
   ```

##### 引入vue-amap

> 在`main.js`文件中引入并配置`vue-amap`

```javascript
import VueAMap from 'vue-amap';
Vue.use(VueAMap);
VueAMap.initAMapApiLoader({
    key:"你在高德平台申请的key",
    plugin:['AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType', 'AMap.PolyEditor', 'AMap.CircleEditor'],   //插件
    v:"1.4.4"  //版本号，默认高德sdk版本为1.4.4，可自行修改
})
```

> 在你的组件中写入如下代码，即可引入地图

```javascript
<template>
    <div class="map" id="map">
        <el-amap vid="amapDemo" :center="center" :zoom="zoom" class="amap-demo">
        </el-amap>
    </div>
</template>
<style scoped>
    #map{
		width:500px;
		height:500px;
	}
</style>
<script>
    export default{
		data(){
            return{
                zoom:12,
                center: [121.59996, 31.197646],
            }
		}
	}
</script>
```

##### 对了，差点忘记，在`main.js`中引入之后如果在函数中输出`VueAMap`为undefined，这时在`main.js`中写下：

```javascript
Vue.prototype.VueAMap = VueAMap;
```

##### `vue-amap`能够抛开高德原生 SDK 覆盖大多数场景，但对于部分定制化程度较高的场景而言，可能还是需要引入高德原生 SDK 来支持。下面介绍如何在 vue-amap 中使用高德 SDK。[官方文档](https://elemefe.github.io/vue-amap/#/zh-cn/introduction/compatible)

```javascript
// 在上面代码的基础上加上点东西
// 在el-amap中加上amapManager
<el-amap vid="amapDemo" :amap-manager="amapManager" :center="center" :zoom="zoom" class="amap-demo">
// 然后在script的开头加上
    // npm方式
import { AMapManager } from 'vue-amap';
let amapManager =new AMapManager();
	//CDN方式
let amapManager = new VueAMap.AMapManager();
// 在data中加上
amapManager,
```

对于大多数 `vue-amap` 组件，都有 `init` 这个 `event`，参数为高德的实例，通过这样暴露高德实例的方式，开发者能够非常自由地将原生 SDK 和 vue-amap 结合起来使用。

这里以 `el-amap` 组件举例。`el-amap` 比较特殊，它同时还支持一个 `amap-manager` 属性，通过这个属性，可以在任何地方拿到高德原生 `AMap.Map` 实例。



*若涉及到高德原生 AMap 需要注意的点：*

- 确保 `vue-amap` 的导入名不是 `AMap`，推荐 `import VueAMap from 'vue-amap'` 避免和高德全局的 `AMap` 冲突。
- 若 `eslint` 报错 `AMap is undefined` 之类的错误。请将 `AMap` 配置到 `.eslintrc` 的 `globals` 中。

##### 完整代码：

```javascript
<template>
    <div class="map" id="map">
        <el-amap vid="amapDemo" :amap-manager="amapManager" :center="center" :zoom="zoom" class="amap-demo">
    </div>
</template>
<style scoped>
    #map{
		width:500px;
		height:500px;
	}
</style>
<script>
	import { AMapManager } from 'vue-amap';
	let amapManager =new AMapManager();
    export default{
		data(){
            return{
                zoom:12,
                center: [121.59996, 31.197646],
                amapManager
            }
		}
	}
</script>
```

#### 对于vue-amap，发现在实际的开发安装使用的时候还是存在很多问题，对于开发者不太友好。经过度娘的查找，发现有现成的解决方法(因为需求不同，所以根据需求对作者的代码进行了修改)。[作者源码：View on GitHub](https://github.com/zuley/vue-gaode)

##### 实现思路：

1. 创建一个mapDrape的公共组件
2. 在这个公共组件的created生命周期函数中载入高德地图API
3. API载入完成即开始地图初始化
4. 地图API暴露全局变量window.AMap可以直接使用
5. 监听地图的拖放事件，获得数据后通知自定义事件，对组件外部提供事件接口

- 首先在`src`目录下创建`config`文件夹用于存放地图所需配置，然后在目录中写入`config.js`

  ```javascript
  // 高德地图 key
  export const MapKey = '你在高德地图开发平台申请的key'
  // 地图限定城市
  export const MapCityName = '全国'
  ```

- 在`src`目录下创建`utils`文件夹，用于在页面中写入`script`标签对及其内容，在目录中写入`remoteLoad.js`

  ```javascript
  export default function remoteLoad (url, hasCallback) {
    return createScript(url)
    /**
     * 创建script
     * @param url
     * @returns {Promise}
     */
    function createScript (url) {
      let scriptElement = document.createElement('script')
      document.body.appendChild(scriptElement)
      let promise = new Promise((resolve, reject) => {
        scriptElement.addEventListener('load', e => {
          removeScript(scriptElement)
          if (!hasCallback) {
            resolve(e)
          }
        }, false)
  
        scriptElement.addEventListener('error', e => {
          removeScript(scriptElement)
          reject(e)
        }, false)
  
        if (hasCallback) {
          window.____callback____ = function () {
            resolve()
            window.____callback____ = null
          }
        }
      })
  
      if (hasCallback) {
        url += '&callback=____callback____'
      }
  
      scriptElement.src = url
  
      return promise
    }
  
    /**
     * 移除script标签
     * @param scriptElement script dom
     */
    function removeScript (scriptElement) {
      document.body.removeChild(scriptElement)
    }
  }
  ```

- 创建公共组件`mapDrap.vue`

  > 载入API的方式类似于jQuery的脚本加载，这里用的别人封装好的一个加载方法

  ```javascript
  <!--
    描述：拖放地图组件，默认尺寸是 500 * 300
  
    接收属性参数：
      lat: 纬度
      lng: 经度
  
    自定义事件：
      drag: 拖放完成事件
  
    示例：
      <mapDrag @drag="dragMap" lat="22.574405" lng="114.095388"></mapDrag>
  -->
  <template>
    <div class="m-map">
      <div id="js-container" class="map" style="font-size: 0.2rem"></div>
    </div>
  </template>
  
  <script>
  import remoteLoad from '../utils/remoteLoad.js'
  import { MapKey, MapCityName } from '../config/config'
  export default {
    // 获取父组件值
    props: ['province', 'city','counties'],
    data () {
      return {
        flag:true,
        searchKey: '',
        placeSearch: null,
        dragStatus: false,
        AMapUI: null,
        AMap: null,
        Vdistrict:"",
        district:null,
        polygons:[],
        map:null,
        geocoder:null,
        markers:[],
        infoWindow:null,
        provinceList:[],
        cityList:[],
        districtList:[]
  
      }
    },
    watch: {
      // 监听父组件值
      province: {
        handler(newVal) {
          this.provinceList = [];
          for (let i=0;i<newVal.length;i++){
            this.provinceList.push(newVal[i].name)
          }
          this.geoCode(this.provinceList,"province",newVal)
        },
        deep: true
      },
      city: {
        handler(newVal) {
          this.cityList = [];
          for (let i=0;i<newVal.length;i++){
            this.cityList.push(newVal[i].name)
          }
          this.geoCode(this.cityList,"city",newVal)
        },
        deep: true
      },
      counties: {
        handler(newVal) {
          this.districtList = [];
          for (let i=0;i<newVal.length;i++){
            this.districtList.push(newVal[i].name)
          }
          this.geoCode(this.districtList,"district",newVal);
        },
        deep: true
      }
    },
    methods: {
        // 在地图上添加marker并添加鼠标移入显示信息框
      geoCode(list,nowCity,newVal) {
        if(!this.geocoder){
          this.geocoder = new AMap.Geocoder({
            city: "全国", //城市设为北京，默认：“全国”
          });
        }
        this.infoWindow = new AMap.InfoWindow({offset: new AMap.Pixel(0, -30)});
        this.map.remove(this.markers);
        this.markers = [];
        // var addresses  = document.getElementById('address').value.trim().split('\n');
        let _this = this;
        var infoClose = function(e) {
          _this.infoWindow.close(_this.map, e.target.getPosition());
        };
        var infoOpen = function(e) {
          _this.infoWindow.setContent(e.target.content);
          _this.infoWindow.open(_this.map, e.target.getPosition());
        };
        _this.geocoder.getLocation(list, function(status, result) {
          if (status === 'complete'&&result.geocodes.length) {
            for(var i=0;i<result.geocodes.length;i+=1){
              var  marker = new AMap.Marker({
                position: result.geocodes[i].location
              });
              marker.content = '<div>地点：' + result.geocodes[i].addressComponent[nowCity]  + '</div>' + '<div>项目总数：' + newVal[i].count + '</div>';
              marker.on('mouseover', infoOpen);
              marker.on('mouseout', infoClose);
              _this.markers.push(marker);
            }
            _this.map.add(_this.markers);
            _this.map.setFitView(_this.markers);
          }
        });
      },
      // 实例化地图
      initMap () {
        // 加载PositionPicker，loadUI的路径参数为模块名中 'ui/' 之后的部分
        let AMapUI = this.AMapUI = window.AMapUI;
        let AMap = this.AMap = window.AMap;
        AMapUI.loadUI(['misc/PositionPicker'], PositionPicker => {
          let mapConfig = {
            resizeEnable: true,
            mapStyle: 'amap://styles/blue',
            center: [116.30946, 39.937629],
            zoom: 3,
          };
          this.map = new AMap.Map('js-container', mapConfig);
          this.flag = false;
          var Vdistrict = this.$store.state.district.toString();
          var district = null;
          var polygons=[];
          // this.$options.methods.drawBounds(map,Vdistrict,district,polygons);
          // this.$options.methods.getWeather(map,Vdistrict)
        })
      },
      drawBounds(map,Vdistrict,district,polygons) {
        //加载行政区划插件
        if(!district){
          //实例化DistrictSearch
          var opts = {
            subdistrict: 0,   //获取边界不需要返回下级行政区
            extensions: 'all',  //返回行政区边界坐标组等具体信息
            level: 'district'  //查询行政级别为 市
          };
          district = new AMap.DistrictSearch(opts);
        }
        //行政区查询
        district.search(Vdistrict, function(status, result) {
          map.remove(polygons);   //清除上次结果
          polygons = [];
          var bounds = result.districtList[0].boundaries;
          if (bounds) {
            for (var i = 0, l = bounds.length; i < l; i++) {
              //生成行政区划polygon
              var polygon = new AMap.Polygon({
                strokeWeight: 1,
                path: bounds[i],
                fillOpacity: 0.4,
                fillColor: '#80d8ff',
                strokeColor: '#0091ea'
              });
              polygons.push(polygon);
            }
          }
          map.add(polygons);
          map.setFitView(polygons);//视口自适应
        });
      }
    },
    async created () {
      // 已载入高德地图API，则直接初始化地图
      if (window.AMap && window.AMapUI) {
        this.initMap()
      // 未载入高德地图API，则先载入API再初始化
      } else {
        await remoteLoad(`https://webapi.amap.com/maps?v=1.4.13&key=${MapKey}`);
        await remoteLoad('https://webapi.amap.com/ui/1.0/main.js');
        this.initMap()
      }
    }
  }
  </script>
  
  <style lang="css">
  .m-map .map{ width: 100%; height: 100%; }
  </style>
  
  ```

- 在`APP.vue`中引入注册这个公共组件，在页面中使用这个组件

  ```javascript
  <template>
  	<div class="map" id="map">
      	<mapDrag class="mapbox"></mapDrag>
      </div>
  </template>
  <script>
      import mapDrag from '../../components/mapDrag'
      import urlConfig from '../../assets/js/url.config.js'
      export default {
          components: {
              mapDrag
          },
          data() {
              return {
                  times:'',
                  dateTime:"",
                  datas_one:[],
                  datas_two:[],
                  obj:{},
                  myUrl:"/",
                  cur:0,
                  flag:true,
                  url:urlConfig.Data(),
                  project:{},
                  device:[],
                  projectMessage:{},
              }
          },
          mounted(){
              
          },
  
          methods: {
              
          }
      }
  </script>
  ```

  