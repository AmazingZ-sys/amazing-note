#### 在vue中使用百度数据可视化解决方案Echarts，通过学习发现也有很多种使用方式，暂时先使用第二种方案

1. ##### 引用官方`Echarts`（vue cli3.0结合echarts3.0）

   - 安装`echarts`

     ```javascript
     npm install echarts --save
     ```

   - 在`main.js`中引入并挂载到`vue`的`prototype`上

     ```javascript
     import echarts from "echarts"
     Vue.prototype.$echarts = echarts
     ```

   - 在页面中创建一个放图表的容器

     ```javascript
     <div id="charts1"></div>
     ```

   - 按需引入需要的`echarts`组件

     ```javascript
     import "echarts/lib/chart/line"
     import "echarts/lib/chart/pie"
     ```

   - 在`data`中配置好echarts的配置项和数据

     ```javascript
     option:{
         ...
     }
     ```

   - 在生命周期函数`mounted()`中初始化图表，在对应的方法中获取`echarts`节点并渲染

     ```javascript
     // 在mounted中调用初始化函数
     this.drawLine()
     // 在函数中获取节点并渲染
     this.charts1 = this.$echarts.init(document.getElementById("charts1"))
     this.charts1.setOption(this.option)
     ```

   - 在屏幕大小发生改变时，我们得重新渲染图表（调用`resize`方法）

     ```javascript
     window.onresize = ()=>{
         this.charts1.resize()
     }
     ```

     

2. ##### 有大佬对`vue-echarts`进行了再封装为`vue-echarts-v3`，[官方文档](https://www.npmjs.com/package/vue-echarts-v3)

   安装`vue-echarts-v3`

   ```javascript
   npm install --save echarts vue-echarts-v3
   ```

   使用`vue-echarts-v3`

   > 值得注意的是在`vue`某一组件中导入了全部的`echarts`图表和组件，它会注册到全局，在其他组件中也全部导入，打包之后它只会引用一次，并不会造成资源浪费

   ```javascript
   // 导入全部组件和echarts图表
   import IEcharts from 'vue-echarts-v3/src/full.js';
   // 分块导入
   import IEcharts from 'vue-echarts-v3/src/lite.js';
    
   // import 'echarts/lib/chart/line';
   import 'echarts/lib/chart/bar';
   // import 'echarts/lib/chart/pie';
   // import 'echarts/lib/chart/scatter';
   // import 'echarts/lib/chart/radar';
    
   // import 'echarts/lib/chart/map';
   // import 'echarts/lib/chart/treemap';
   // import 'echarts/lib/chart/graph';
   // import 'echarts/lib/chart/gauge';
   // import 'echarts/lib/chart/funnel';
   // import 'echarts/lib/chart/parallel';
   // import 'echarts/lib/chart/sankey';
   // import 'echarts/lib/chart/boxplot';
   // import 'echarts/lib/chart/candlestick';
   // import 'echarts/lib/chart/effectScatter';
   // import 'echarts/lib/chart/lines';
   // import 'echarts/lib/chart/heatmap';
    
   // import 'echarts/lib/component/graphic';
   // import 'echarts/lib/component/grid';
   // import 'echarts/lib/component/legend';
   // import 'echarts/lib/component/tooltip';
   // import 'echarts/lib/component/polar';
   // import 'echarts/lib/component/geo';
   // import 'echarts/lib/component/parallel';
   // import 'echarts/lib/component/singleAxis';
   // import 'echarts/lib/component/brush';
    
   import 'echarts/lib/component/title';
    
   // import 'echarts/lib/component/dataZoom';
   // import 'echarts/lib/component/visualMap';
    
   // import 'echarts/lib/component/markPoint';
   // import 'echarts/lib/component/markLine';
   // import 'echarts/lib/component/markArea';
    
   // import 'echarts/lib/component/timeline';
   // import 'echarts/lib/component/toolbox';
    
   // import 'zrender/lib/vml/vml';
   ```

   在`vue`组件中使用示例

   > `resizable`属性是一大亮点，我们不必再进行麻烦的`onresize`事件监听，但是注意包裹图表组件的容器大小不能是定值，要不然这个属性会失效，容器大小设置`width:100%;height:100%;`

   ```javascript
   <template>
     <div class="echarts">
       <IEcharts
         :option="bar"
         :loading="loading"
   	  ：resizable="true"     // 加上这个属性时，在页面onresize的时候图表大小也会自适应
   	  :style="{'width':'100%','height':'100%'}"   // 组件对容器大小自适应
         @ready="onReady"
         @click="onClick"
       />
       <button @click="doRandom">Random</button>
     </div>
   </template>
    
   <script type="text/babel">
     import IEcharts from 'vue-echarts-v3/src/full.js';
     export default {
       name: 'view',
       components: {
         IEcharts
       },
       props: {
       },
       data: () => ({
         loading: true,
         bar: {
           title: {
             text: 'ECharts Hello World'
           },
           tooltip: {},
           xAxis: {
             data: ['Shirt', 'Sweater', 'Chiffon Shirt', 'Pants', 'High Heels', 'Socks']
           },
           yAxis: {},
           series: [{
             name: 'Sales',
             type: 'bar',
             data: [5, 20, 36, 10, 10, 20]
           }]
         }
       }),
       methods: {
         doRandom() {
           const that = this;
           let data = [];
           for (let i = 0, min = 5, max = 99; i < 6; i++) {
             data.push(Math.floor(Math.random() * (max + 1 - min) + min));
           }
           that.loading = !that.loading;
           that.bar.series[0].data = data;
         },
         onReady(instance, ECharts) {
           console.log(instance, ECharts);
         },
         onClick(event, instance, ECharts) {
           console.log(arguments);
         }
       }
     };
   </script>
    
   <style scoped>
     .echarts {
       width: 400px;
       height: 400px;
     }
   </style>
   ```

   理论上讲在echarts图表库中能使用的配置项都能在这使用，没有仔细进行深度挖掘。。。。。

3. ##### 使用`vue-echarts`

   安装`vue-echarts`

- 使用`npm`安装（推荐）

  ```javascript
  npm install echarts vue-echarts
  ```

- `CDN`

  > 包含`echarts`和`vue-echarts`在你的`html`文件中，如下所示

  ```javascript
  < script src = “ https://cdn.jsdelivr.net/npm/echarts@4.1.0/dist/echarts.js ” > < / script > 
  < script src = “ https://cdn.jsdelivr.net/npm/vue-echarts@4.0.0 ” > < / script > 
  ```

使用`vue-echarts`

- 在`main.js`中引入

  ```javascript
  import ECharts from 'vue-echarts/components/ECharts.vue'
  
  //引入所有表 
  require(‘echarts’);
  
  // 手动引入 ECharts 各模块来减小打包体积
  import 'echarts/lib/chart/bar'
  import 'echarts/lib/chart/line'
  import 'echarts/lib/component/tooltip'
  import 'echarts/lib/component/polar'
  import 'echarts/lib/component/legend'
  import 'echarts/lib/component/title.js'
  
  // 注册组件以便在项目中引用
  Vue.component('charts', ECharts);
  ```

- 在你想要使用的组件`xxx.vue`中引入组件

  > 注意包裹`echarts`组件的容器大小单位必须为`px`  要不然就会发生图表组件大小异常的情况

  ```javascript
  <template>
      <div id="echartsBox">
          <charts :options="option" style="width:100%;height:100%"></charts>
      </div>
  </template>
  <script>
      export default{
          data(){
              return{
                  option: {
                      xAxis: {
                          type: 'category',
                          boundaryGap: false,
                          data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
                      },
                      yAxis: {
                          type: 'value'
                      },
                      series: [{
                          data: [820, 932, 901, 934, 1290, 1330, 1320],
                          type: 'line',
                          areaStyle: {}
                      }]
                  },
              }
          }
      }
  </script>
  ```


