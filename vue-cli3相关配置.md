

#### 在`vue.config.js`中做如下配置：

```javascript
const webpack = require('webpack');
module.exports = {
    filenameHashing:process.env.NODE_ENV === 'production'?false:true,   // 打包输出文件名是否hash
    publicPath: process.env.NODE_ENV === 'production'? './': '/',
    productionSourceMap:false,
    outputDir: 'dist', // 构建输出目录
    assetsDir: './resources/vue_pit/', // 静态资源目录 (js, css, img, fonts)
    // 服务器设置
    devServer: {
        host: '127.0.0.1',    // 根路径
        port: 9099,    // 启动端口号
        https: false,    //是否采用https协议
        hotOnly: false,   //只采用热加载？
        proxy: {    // 跨域的相关配置
            '/api/*': {
            target: 'http://127.0.0.1:8080',
            ws: true,
            changeOrigin: true,
            pathRewrite: {
                '^/api': ''
            }
          },
        }
      },
    // 在vue-cli3中安装并全局引入jQuery
    configureWebpack: {
        plugins: [
            new webpack.ProvidePlugin({
                jQuery: 'jquery',
                $: 'jquery'
            })
        ]
    },
    // 路由懒加载
    chainWebpack: config => {
        // 修改prefetch：
        config.plugin('prefetch').tap(options => {
            options[0].fileBlacklist = options[0].fileBlacklist || [];
            options[0].fileBlacklist.push(/myasyncRoute(.)+?\.js$/);
            return options
        });
    },
};

```

#### 路由懒加载在`router`中的使用：

```javascript
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router);

const router =  new Router({
  mode:"hash",
  routes: [
    {
      path: '/pc/statics',
      name: 'statics',
      component: () => import('@/components/statics.vue'),
    },
    {
      path: '/pc/video',
      name: 'video',
      component: () => import('@/components/video.vue'),
    },
    {
      path: '/pc/information',
      name: 'information',
      component: () => import('@/components/information.vue'),
    },
    {
      path: '/pc/home',
      redirect: '/',
    },
    {
      path: '/home',
      redirect: '/',
    },
    {
      path: '/super/data',
      name: 'superData',
      component: () => import('@/components/data.vue'),
    },
    {
      path: '/super/video',
      name: 'superVideo',
      component: () => import('@/components/video.vue'),
    },
    {
      path: '/information',
      name: 'superInformation',
      component: () => import('@/components/information.vue'),
    },
    {
      path: '/pc/data',
      name: 'data',
      component: () => import('@/components/data.vue'),
    },
  ]
});
router.onError((error) => {
  const pattern = /Loading chunk (\d)+ failed/g;
  const isChunkLoadFailed = error.message.match(pattern);
  const targetPath = router.history.pending.fullPath;
  if(isChunkLoadFailed){
    router.replace(targetPath);
  }
});
export default router
```

