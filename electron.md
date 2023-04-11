## Electron初探

#### Electron是什么？

- `Electron`是使用` JavaScript`，`HTML` 和` CSS` 构建跨平台的桌面应用程序
- `Electron`是由`github`开发的开源框架
- 它允许开发者使用web技术开发跨平台的桌面应用
- `Electron = Chromium + Node.js + Native API`
  - Chromium：提供了强大的ui能力，利用强大的web生态来开发界面
  - Node.js：让 Electron有了底层才做能力，比如读写能力，并且可以使用大量的开源包来完成项目的开发
  - Native API：让Electron有了跨平台和桌面端的原生能力，比如有同意的原生界面、窗口、托盘

#### 什么时候使用Electron

- 公司没有专门的桌面应用开发者，而需要前端兼顾来进行开发时，用Electron就是一个不错的选择
- 一个应用同时开发Web端和桌面端的时候，那使用Electron来进行开发就对了
- 开发一些效率工具，比如说我们的VSCode，比如说一些API类的工具，用Electron都是不错的选择

#### 那些使用Electron开发的著名应用

- VSCode ： 程序员最常用的开发者工具
- Atom : 是Github开发的文本编辑器
- slack ： 聊天群组 + 大规模工具集成 + 文件整合 + 搜索的一个工具
- 百度云网盘（Mac版）：大家都懂的

#### 开始构建Electron

1. 新建文件夹(命名记得不要出现中文)，初始化项目

   ```javascript
   npm init // 初始化文件夹
   ```

2. 安装electron

   ```javascript
   npm install --save-dev electron
   ```

   如果安装的时候遇到什么问题(大部分报错或太慢)可以看看npm是否切换了淘宝镜像

   ```javascript
   npm config set registry https://registry.npm.taobao.org
   ```

   也可以配置一下electron镜像

   ```javascript
   npm config set ELECTRON_MIRROR http://npm.taobao.org/mirrors/electron/
   ```

   看看electron的版本号

   ```javascript
   npx electron -v
   ./node_modules/.bin/electron -v
   ```

   启动Electron

   ```javascript
   ./node_modules/.bin/electron
   ```

   ![](https://tva1.sinaimg.cn/large/0081Kckwly1gk5et6wdt3j318c0u0gqj.jpg)

   出现该窗口说明启动成功

3. 根目录下创建main.js文件(主进程) electron主进程

   ```javascript
   const { app, BrowserWindow } = require('electron');
   let mainWindow = null;
   
   app.on('ready', () => {
       mainWindow = new BrowserWindow({    // 创建和控制浏览器窗口
           width: 800,                     // 窗口宽度
           height: 600,                    // 窗口高度
           webPreferences: {               // 网页功能设置
               nodeIntegration: true,      // 是否在node工作器中启用工作集成默认false
               enableRemoteModule: true,   // 是否启用remote模块默认false
           }
       });
       mainWindow.loadFile('index.html');  // 加载页面
       mainWindow.on('close', () => {      // 监听窗口关闭
           mainWindow = null
       })
   
   })
   ```

4. 根目录下创建index.html （需要加载的页面）

   ```javascript
   <html lang="en">
   
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
   </head>
   
   <body>
       <button id="btn">
           点击弹窗
       </button>
       <script src="./render/index2.js"></script>
   </body>
   
   </html>
   ```

5. 修改paceage.json文件中的main的值，`main`表示需要启动的脚本， 将scripts 的test 修改为自己需要的名字或者只修改test 的值

   ```javascript
   {
     "name": "test",
     "version": "1.0.0",
     "description": "",
     "main": "main.js",
     "scripts": {
       "start": "electron .",
     },
     "author": "",
     "license": "ISC",
     "dependencies": {
     }
   }
   ```

6. 运行第一个Electron应用

   ```javascript
   npm run start
   // 或者(在全局环境安装了electron)
   electron .
   ```

#### Electron的运行流程

1. 读取package.json文件中的入口文件，这里就是我们的main.js
2. main.js中我们引入`electron` 创建了渲染进程
3. index.html就是应用页面的布局和样式
4. 使用IPC在主进程执行任务并获取信息

>在electron中分为主进程和渲染进程，上述文件main.js就是主进程，index.html就是渲染进程
>
>每个Electron中的web页面运行在它自己的渲染进程中
>
>一个主进程控制着多个渲染进程
>
>![](https://tva1.sinaimg.cn/large/0081Kckwly1gk5g2ostw9j30ku0dwwf0.jpg)



#### 在Electron中使用node.js

1. 我们在render目录下创建一个nameList.txt文件，里面随便写点东西

2. 接着在render中创建一个js文件夹在js文件夹内创建index.js文件

3. 在index.html中添加一个按钮，添加一个div标签(用于显示内容)，并且引入`index.js`

4. 接着在`index.js`中获取按钮元素，读取test.txt文件内容

5. 代码如下：

   ```javascript
   // index.html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
   </head>
   <body>
   <h3>hello world</h3>
   <button id="btn">点击获取文件信息</button>
   <div id="filesData"></div>
   <script src="./render/js/index.js"></script>
   </body>
   </html>
   
   // index.js
   const fs = require('fs');
   const path = require('path')
   
   const btnRead = document.querySelector('#btn');
   const filesDaata = document.querySelector('#filesData');
   
   
   window.onload = ()=>{
       btnRead.onclick = () => {
           fs.readFile(path.join(__dirname+'/render', 'nameList.txt'),(err,data)=>{
               console.log(data);
               filesDaata.innerHTML = data
           })
       }
   }
   ```

Electron的remote模块

> 说`Remote`之前我们要明确一点，当我们知道了Electron是分`主进程`、`渲染进程`后，还需要知道Electron的API方法和模块也是分`主进程`、`渲染进程`
>
> Remote是渲染进程（web页面）和主进程通信（IPC）提供一种简单的方法

1. 在index.html中写上一个按钮，通过点击事件创建一个新的窗口

2. 创建一个`yellow.html`用作新建窗口的渲染进程

   ```html
   // index.html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
   </head>
   <body>
   <h3>hello world</h3>
   <button id="btn">点击获取文件信息</button>
   <button id="newBtn">打开新窗口</button>
   <div id="filesData"></div>
   <script src="./render/js/index.js"></script>
   </body>
   </html>
   
   
   // yellow.html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Document</title>
   </head>
   <body>
       我是新建的窗口
   </body>
   </html>
   ```

   js代码如下

   ```javascript
   const fs = require('fs');
   const path = require('path')
   const BrowserWindow = require('electron').remote.BrowserWindow;
   
   const btnRead = document.querySelector('#btn');
   const newBtn = document.querySelector('#newBtn');
   const filesDaata = document.querySelector('#filesData');
   
   let newWindow = null
   
   
   window.onload = ()=>{
       btnRead.onclick = () => {
           fs.readFile(path.join(__dirname + '/render', 'nameList.txt'),(err,data)=>{
               console.log(data);
               filesDaata.innerHTML = data
           })
       }
   
       newBtn.onclick = ()=>{
           newWindow = new BrowserWindow({
               width: 500,
               height:500,
           })
           newWindow.loadFile(path.join(__dirname + '/render', 'yellow.html'));
           newWindow.on('close',()=>{
               newWindow = null;
           })
       }
   }
   ```

   我们看看`主进程`中的一段代码在`webPreferences` 里写了一句`enableRemoteModule: true`

   这是`Electron`版本更新后修改的，默认不启用remote模块需要手动打开,别忘记了

#### Electron创建菜单和基本使用

- 编写菜单模板：在`Electron`中编写菜单，需要先建立一个模板，这个目标很类似我们`JSON`或者类的数组。我们打开项目，在项目的根目录下新建一个文件夹`main`，意思是主进程中用到的代码我们都写到这里

  ```javascript
  const {Menu} = require('electron');
  
  const  temlpate = [
      {
          label: '第一项',
          submenu:[
              {label: '1.1'},
              {label: '1.2'}
          ]
      },
      {
          label: '第二项',
          submenu:[
              {label:'2.1'},
              {label:'2.2'}
          ]
      }
  ]
  
  const menu = Menu.buildFromTemplate(temlpate);
  Menu.setApplicationMenu(menu);
  ```

- 然后再打开主进程`main.js`文件，在`ready`生命周期中，直接加入下面的代码，就可以实现自定义菜单了

  ```javascript
  require('./render/js/menu')
  ```

- 使用菜单打开新窗口：有了菜单之后，可以在菜单中加入`click`事件，代码如下:

  ```javascript
  const {Menu, BrowserWindow} = require('electron');
  const path = require('path')
  
  const  temlpate = [
      {
          label: '第一项',
          submenu:[
              {
                  label: '1.111111111111111111111',
                  accelerator: 'n',
                  click: () => {
                      let win = new BrowserWindow({
                          width: 300,
                          height: 300,
                      });
                      win.loadFile( 'red.html');
                      win.on('close',()=>{
                          win = null;
                      })
                  },
              },
              {label: '1.222222222222222222222'}
          ]
      },
      {
          label: '第二项',
          submenu:[
              {label:'2.1'},
              {label:'2.2'}
          ]
      }
  ]
  
  const menu = Menu.buildFromTemplate(temlpate);
  Menu.setApplicationMenu(menu);
  ```

  加上`accelerator:'crtl+n'`实现快捷键打开

  

  #### Electron制作右键菜单

  > 右键菜单的响应事件是写在渲染进程中的，也就是写在`index.html`中的，所以要是使用，就用到到`remote`模块进行操作了

  `Index.html`

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
  </head>
  <body>
  <h3>hello world</h3>
  <button id="btn">点击获取文件信息</button>
  <button id="newBtn">打开新窗口</button>
  <div id="filesData"></div>
  <script src="./render/js/index.js"></script>
  </body>
  </html>
  ```

  `Index.js`

  ```javascript
  // 在index.js中添加以下事件
  const { remote } = require('electron');
  
  const rightTemplate = [
      {label:'粘贴'},
      {label:'复制'},
  ]
  const menu = remote.Menu.buildFromTemplate(rightTemplate);
  
  window.addEventListener('contextmenu',(e)=>{
      // 阻止当前窗口默认事件
      e.preventDefault();
      //把菜单模板添加到右键菜单
      menu.popup({window:remote.getCurrentWindow()})
  })
  ```

  #### 打开Electron的网页调试器

  >由于我们已经定义了顶部菜单，没有了打开调试模式的菜单了，这时候可以使用程序来进行打开。在主进程中加入这句代码就可以了

  ```javascript
  mainWindow.webContents.openDevTools()
  ```

  #### 通过链接打开浏览器

  > 在渲染进程中默认加入一个a标签，进行跳转默认是直接在窗口中打开，而不是在浏览器中打开的
  >
  > 使用shell在浏览器打开：给a标签添加事件，打开浏览器跳转

  ```javascript
  // 通过链接打开浏览器
  const { shell } = require('electron');
  
  const aHref = document.getElementById('aHref');
  
  aHref.onclick = (e)=>{
      // 阻止默认事件 因为默认是在应用中打开;
      e.preventDefault();
      // 获取链接
      const href = aHref.getAttribute('href');
      // 浏览器中打开
      shell.openExternal(href)
  }
  ```

  #### 嵌入网页和打开子窗口

  >我们可以使用`BrowserView`嵌入网页到应用中

  ```javascript
  // 在main.js主进程中加入嵌入网页的代码
  const viewWindow = new BrowserView(); // 定义实例
  mainWindow.setBrowserView(viewWindow); // 主窗口设置viewWindw可用
  viewWindow.setBounds({x:0,y:150,width: 600,height:600});
  viewWindow.webContents.loadURL('https://www.baidu.com')
  ```

  上面通过BowserView嵌入网页， 下面来看看window.open打开子窗口

  ```javascript
  // 打开子窗口
  const btn = document.getElementById('child');
  btn.onclick = ()=>{
      window.open('https://www.wondercv.com')
  }
  ```

  #### Electron父子窗口之间的通信

  ##### 子窗口向父窗口传递消息：window.opener.postMessage

  `window.opener.postMessage(message,targetOrigin)`,是将消息发送给指定来源的父窗口，如果未指定来源则发送给`*`，即所有窗口

  1. message : 传递的消息，是`String`类型的值
  2. targetOrigin : 指定发送的窗口

  ##### 父窗口接收消息：window.addEventListener

  ```javascript
  window.addEventListener('message',(msg)=>{
      let mytext = document.querySelector('#mytext')
      mytext.innerHTML = JSON.stringify(msg)
  })
  
  ```

  #### 打包桌面应用

  安装`electron-builder`

  ```javascript
  npm install electron-builder --save-dev
  ```

  配置脚本命令

  ```javascript
  "build": "electron-builder --win --x64"
  ```

  运行打包命令

  ```javascript
  npm run build
  ```

  

  

  

  

