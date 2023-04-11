## Vue框架：

> 是一种MV VM的开发方式，基于node.js

- 选择容器，确定Vue的接管范围

```javascript
new Vue({
	el:''，  //接管范围  选择器
data:{        //数据
	one:'',
	two:''   
}，
mounted(){      //生命周期函数，解决异步，页面加载完成触发
    fetch("data.txt").then(function(e){
        return e.json()
    }).then((e)=>{
        this.aa=e
    })
},
methods:{  //事件触发的函数写在这里面
aa(){
	return 1
}
},
computed:{  //只会在和他有关联的数据改变时执行  在视图中呈现时不需要加（）
            result:function () {
                return this.one*1+this.two*1
            }
        },
watch:{  //监控
            one(one,two){  //要监控的数据--one  参数one表示改变后的数据，two表示改变前的数据
                this.result=this.one*1+this.two*1 //one改变时要执行的逻辑
            },
            two(one,two){
                this.result=this.one*1+this.two*1
            }
        }
 })
//数据呈现
 在表单里用v-model指令 在其他模块中用{{}}
添加事件：v-on:事件名="函数"  相当于@
添加类：v-bind:class="{类名：条件}"   条件为真就加进类名  为假不加进去
```

### 组件化开发：

```javascript
new Vue({
	el:''，  //接管范围  选择器
    data:{        //数据
        aa:[]   
    }，
    mounted(){      //生命周期函数，解决异步
        fetch("data.txt").then(function(e){
            return e.json()
        }).then((e)=>{
            this.aa=e
        })
    }
```

```javascript
var obj={}
Object.defineproperty(obj,"name",{
    value:100,
    writble:true,    //可写，可设置
    configurable:true,   //
    enmuerable:true,
    set:function(){
        
    },
    get:function(){
        
    }
})   //value、writable  不可与set、get   同时出现   否则报错
```

vue2+

- npm install -g vue-cli

- vue init webpack aaa(目录)    y   n   n   n      cd**     npm  

vue3+

- npm install -g @vue/cli
- vue create bbb(目录)
- babel  vuex  router  空格选中（手动预设）
- ​                     n
- cd   

uri    互联网中资源的定位方式

url    地址的方式访问唯一的互联网资源

标准格式：tcp/ip协议簇（http/https/svn）：// + [用户名：密码@]主机名（）+ 端口/ + 路径/ + 文件名/ + ？查询字符串 + #锚链接

#### 操作地址栏：

```javascript
localtion.href  //获取地址栏
localtion.hash  //获取锚链接
```

路由：控制器

#### 定义路由：

- 定义组件
- 定义路由
- 将路由绑定

#### cli

- assets  资源
- components   组件
- views    

#### 跨域访问

- jsonp   快捷   诸多限制        （script   标签对）
  - 局限性，自己有权限操作的两个域名  或者对方配合的服务
- 代理方式   流程复杂  几乎没有限制
  - 效率慢  开发慢
  - 原理：后台语言具有客户端的能力
  - 应用：爬虫，动态跨域获取信息

#### 登录/注册

- hash 算法加密   不可逆的加密方式
- sql  注入  “ or  1=1 ; --          sql   注释  #   或者   --
- 前台的验证    validate
- session

axure    可抛弃的原型

努力是有目标的自觉奋斗   而忙碌则是毫无目的的被人牵着走