### jQuery:

> 本质：JavaScript框架
>
> 宗旨：write Less   do More
>
> 作用：优化js的操作、dom操作、页面交互效果的操作、事件的操作、ajax、动效
>
> 特性：隐式循环，链式调用
>
> 1、将原生js的单个化操作变成批量操作 
>
> 2、将原生的复杂的查询方式（兼容）简单化
>
> 3、将原生逐步操作的方式变成链式的操作
>
> 4、将原生操作复杂的逻辑，提供简单的接口
>
> 目的是只专注于业务的逻辑，语法的问题

#### 常用方法：

- `.toArray()`       将jQuery对象数组转化为原生数组

#### jQuery核心函数：

- 选择器，返回jQuery对象
- 函数，给document 添加 ready 事件
- 传入 dom对象，返回 jQuery对象
- 字符串  "<div></div>"   创建一个新的节点

#### jQuery对象常用方法：

- `.css("样式属性","样式值")`   一个属性的时候
- `.css({})`     多个属性的时候

#### 筛选的方法：

- `eq(index)`   返回对应下标的   jQuery对象
- `get(indx)`   返回对应下标的  dom对象
- `.first()`
- `.last()`
- `hasClass("类名")`    判断 jQuery数组中是否有一个类名为***的对象
- `filter([选择器],[元素],[函数])`    筛选出符合条件的对象
- `.is("选择器")`    判断是否为某个标签
- `.map(fn)`     遍历对象，生成一个新的数组或集合
- `.has("选择器")`     返回一个选中的 jQuery对象
- `.not("选择器")`     从 jQuery数组中删除选中的元素
- `.slice(start,[end])`     截取指定位置的 jQuery对象   end可不写   截取了start到最后
- `.each(function(index,item))`     遍历 jQuery数组   index在前    item在后
- `.index()`        返回 jQuery对象在数组中的下标

#### 操作属性的方法：

- `.attr()`      获取的时候传入一个参数，设置的时候传入两个参数
- `.removeAttr()`
- `prop()`       常用，兼容性强
- `removeProp()`

#### 操作类名的方法：

- `addClass()`        添加一个类名
- `removeClass() `    删除一个类名
- `toggleClass()`     有类名就删除    没有就添加

#### 操作内容的方法：

- `.text()`
- `.html()`
- `.val()`       表单控件的内容

#### jQuery对象元素的位置信息：

- `.offset()`     返回一个位置信息的对象`{top:**,left:**}`     相对于浏览器的位置
- `.position()`     返回一个位置信息的对象`{top:**,left:**}`     相对于定位了的祖先元素的位置
- `scrollTop()`     滚动条在垂直反向的滚动距离
- `scrollLeft()`     滚动条在水平方向的滚动距离

#### 事件对象：

- `e.offsetX()`
- `e.offsetY()`
- `e.pageX()`
- `e.pageY()`
- `e.target()`
- `e.currentTarget()  ==  this`
- `e.stopPropagation()`
- `e.preventDefault()`

#### 事件：

- `noe()`
- `on()` 
- `trigger()`        自动触发事件
- `triggerHandler()`    自动触发事件，并且清除浏览器的默认行为
- `hover(fn,fn)`     前一个移入后一个移出

