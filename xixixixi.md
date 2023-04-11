激活虚拟环境：  .  +  空格  + 目录名/bin/activate

关闭虚拟环境：  deactivate









1. 移动端裂变新方案
2. 简历批注
3. BBS优化迭代
4. 简历投递流程优化







请求：

请求html   之后html会自主请求页面显示需要的东西：

​	link、script、img、background：url()、ajax、视频、音频控件

​	被动的发起请求：a链接、form表单、



document.addEventListener("DOMContentloaded",function(){   //标签加载完就执行  })



数据缓存：

​	将数据提前存储



js的运行原理：   队列控制  

框架的意义：  插件机制

项目的开发方式： 多库共存



make****

send****

xlwt

回调地狱：一直在调用回调函数（控制异步代码时用的回调函数）

队列控制



360*480



#### ajax的文件上传

```
var formData=new FormData()
var file=document.getElementById("file1").files[0]
formData.append('file',file)
var ajax=new XMLHttpRequest()
ajax.onload=(e)=>{
    if(ajax.response=="ok"){
    alert('上传成功！')
    this.$router.push('/course')
    }else {
    alert('上传失败！')
    }
}
ajax.open("post",'/ajax/file/uploading')
ajax.send(formData)
```





#### 运行在服务器：

优点：业务逻辑清晰，工作量少，首页加载速度快

缺点：用户体验差，服务器压力大，不利于协同工作

#### 运行在前端（ajax）：

优点：用户体验性好，减轻服务器的压力，有利于协同工作

缺点：首页加载速度慢，业务逻辑不清晰，工作量大

#### MV VM：

m=>model=>数据   view=>视图=>模板=>html、css

mv vm  由数据到视图  由视图到数据  双向数据绑定   优点>缺点

angular   react   vue

Vue=>node.js

python推导式（用于筛选循环）

ex:

```python
arr=["a","b","c","d"]
arr1=[item for item in arr if item=="c"]  //筛选出列表中元素为"c"  放到新列表中
```







pillow     处理图形图像

easy UI

CKeditor     富文本编辑器    content   ajax上传图片

pip     下载的包放到当前环境下的site_package

npm    下载的包放到根目录的node_modules   找不到目录放到次级目录的node_modules   直到本目录创建node_modules



#### 数据库维护！！



### yeild:生成器

> 将函数返回值变成一个可迭代对象（将返回值装进生成器里）



### 三元表达式：

```python
[on True] if [expression] else [on False]    # 表达式为真，执行[on True] 否则执行[on False]
```

2-21

Q1 能不能把你做过的项目以及工作内容挑一个大型的描述一下
Q2 （针对于响应项目的一系列问题）
Q3 杭州是你的老家吗
Q4 Django和Flask区别
Q5 简述一下Flask的蓝图以及使用的好处
Q6 Django你常用的中间件有哪些
Q7 线程进程很细问了很多，比如区别，使用场景
Q8 python基本语法，比如有什么数据类型，什么类型不能做字典的key
Q9 MySQL的范式
Q10 你有没有什么想问的

2-22

Q1：可变类型和不可变类型（区别）

Q2：字符串的方法

Q3：当表示一个非常大的数字时，使用（科学计数法）

Q4：使用科学计数法所得到的字符类型的数字转换（使用float(str)，int(str)会报错）

2-25

Q1：类中_和__的区别

Q2：`__init__`和`__new__`的区别

Q3：post方式是否可以通过地址栏传输

Q4：mysql中的乐观锁和悲观锁

2-25

Q1：事务有哪些？（隔离性的使用）

Q2：数据库如何优化

Q3：在不进行主从配置的情况下，数据如何同步

Q4：迭代器与生成器

Q5：hash取模

Q6：数据类型（先列举后分类）

Q7：Django的生命周期

Q8：Linux如何查看端口号

Q9：Linux如何查看内存使用情况

##### session存储在服务器的内存













### 全景展示：

> 720yun.com