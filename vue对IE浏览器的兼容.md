#### 360浏览器有极速模式和兼容模式两种区别：

- 极速模式

  > 用最新版本浏览器运行网页，使用chrome内核。

- 兼容模式

  > 以电脑中IE版本运行网页，使用IE内核。

```javascript
//用Meta标签代码让360双核浏览器默认极速模式不是兼容模式
<meta name=”renderer” content=”webkit|ie-comp|ie-stand” /> 

//若页面需默认用极速核，增加标签：
<meta name=”renderer” content=”webkit” /> 

//若页面需默认用ie兼容内核，增加标签：
<meta name=”renderer” content=”ie-comp” /> 

//若页面需默认用ie标准内核，增加标签：
<meta name=”renderer” content=”ie-stand” /> 

```

#### vue对IE浏览器的兼容性解决：

- 使用`vue-cli3`

  - 安装`babel-polyfill`

    ```
    npm install babel-polyfill -s
    ```

  - 对根目录下的`babel.config.js`进行配置

    ```
    module.exports = {
      presets: [
        ['@vue/app', {
            useBuiltIns: 'entry'
        }]
      ]
    }
    ```

  - 在根目录`main.js`第一行加入如下

    ```
    import '@babel/polyfill'
    ```

  - 修改`.browsersListrc`

    ```
    > 1%
    last 2 versions
    not ie <= 10
    ​```
    ```

  - 修改`vue.config.js`配置

    > 在默认情况下`babel-loader`会忽略所有`node_modules`中的依赖包和文件，如果你所用的依赖包并不对IE浏览器做兼容，就需要将依赖包放到`transpileDependencies`中进行显式地转译

    ```
    module.exports = {
      transpileDependencies: []
    }
    ```

- 在`html`中使用`vue.js`

  - 在页面头部引入`vue.min.js`、`polyfill.min.js`、`bluebird.min.js`、`browser.min.js`

    ```
    <script src="/resources/js/polyfill.min.js"></script>    // js中的修补匠
    <script src="/resources/js/bluebird.min.js"></script>    // 使IE浏览器支持promise对象
    <script src="/resources/js/vue.min.js"></script>     // 使用vue.js
    <script src="/resources/js/browser.min.js"></script>     // 使浏览器支持babel
    ```

  - 然后在传统的vue写法上做改变

    ```javascript
    <div id="device_set" style="width: 100%;height: 100%;position: relative;padding-top: 50px;">
    		<div class="container" style="background-color: transparent;width: 100%;display: flex;flex-wrap: wrap">
    			<div class="form-group" style="width: 25%" v-for="item in datas">
    				<label for="" class="control-label col-lg-4" v-text="item.name + '*：'"></label>
    				<div class="col-lg-8">
    					<input type="text" name="code" class="form-control col-lg-6" v-model="item.value" @blur="verify(item)">
    					<span class="form-control col-lg-2" style="color: red;background-color: transparent;border: none;" v-text="item.ver"></span>
    				</div>
    			</div>
    		</div>
    		<div class="btnCon" style="width: 100%;height: auto;background-color: transparent;position: absolute;bottom: 40px;left: 0;display: flex;justify-content: space-around">
    			<button type="button" class="btn btn-info" @click="submits()" v-if="flag">提交数据</button>
    			<button type="button" class="btn btn-info" @click="concel()" v-if="flag">返回列表</button>
    		</div>
    	</div>
    	<script>
    		var vue = new Vue({
    			el:"#device_set",
    			data:function () {
    				return{
    					datas:[],
    					deviceId:"",
    					flag:false
    				}
    			},
    			mounted:function(){
    				window._Dthis = this;
    				var deviceId = window.localStorage.getItem("deviceId");
    				this.deviceId = deviceId;
    				$.ajax({
    					url:"/device/setup/" + deviceId,
    					method:"GET",
    					success: function(e){
    						let tags = e.addon.tags;
    						if (e.data.length>0){
    							_Dthis.datas = e.data.map(function(item){
    								for (let i=0;i<tags.length;i++){
    									if (item.code==tags[i].code){
    										item.name = tags[i].name
    									}
    								}
    								item.ver = "";
    								return item
    							});
    							_Dthis.flag = true;
    						}else {
    							_Dthis.datas = e.addon.tags.filter(function(item){
    								if (item.code.indexOf("Setup")==-1 && item.code!="eq"){
    									item.value = "";
    									item.ver = "";
    									return item
    								}
    							}) ;
    							_Dthis.flag = true;
    						}
    					}
    				});
    			},
    			methods:{
    				concel:function(){
    					layer.close(layer.index)
    				},
    				submits:function(){
    					var params = {};
    					params.deviceId = _Dthis.deviceId;
    					params.setUpList = _Dthis.datas;
    					$.ajax({
    						url: "/device/setup",
    						method: "POST",
    						contentType:"application/json",
    						data: JSON.stringify(params),
    						success:function(e){
    							if (e.success){
    								$.msgBox(e.message);
    								window.localStorage.removeItem("deviceId");
    								layer.close(layer.index)
    							}else {
    								$.msgBox(e.message,"warning");
    								layer.close(layer.index)
    							}
    						}
    					})
    
    				},
    				verify:function(item){
    					if ((!item.value) || ! /^(([^0][0-9]|0)\\.([0-9]{1,4})$)|^([^0][0-9]|0)$/.test(item.value)){
    						item.ver = "请输入整数或不超过4位的小数！"
    					}else {
    						item.ver = ""
    					}
    				}
    			}
    		});
    	</script>
    ```

    