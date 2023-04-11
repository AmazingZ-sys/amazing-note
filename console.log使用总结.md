#### 每天都在使用的`console.log`总结：

1. ##### 基础调用（在任何js代码中都能使用）

   ```javascript
   console.log('123')
   // 123
   console.log('1','2','3')
   // 1 2 3
   console.log('1\n2\n3\n')
   // 1
   // 2
   // 3
   ```

   我们可以通过上面的方式进行单个变量（表达式）、多个变量以及换行输出。

2. 格式化输出

   ```javascript
   console.log('%d + %d = %d',1,2,3);
   // 1 + 2 = 3
   console.log('%d + %d =',1,2,3);
   // 1 + 2 = 3
   ```

   `console.log`所支持的格式标志有：

   | 占位符 |    描述    |
   | :----: | :--------: |
   |   %s   |   字符串   |
   | %d/%i  |    整数    |
   |   %f   |   浮点数   |
   | %o/%O  | object对象 |
   |   %c   |  css样式   |

   前三种格式不用多说，%o和%O都是用来输出object对象，对于普通的object对象输出是没有区别的，在打印dom节点的时候就不一样了：

   - `console.log('%o',document.body);`

     使用%o输出和不使用格式化打印的结果一样，都是打印了这个dom节点的内容、子节点等信息

   - `console.log('%O',document.body);`

     使用%O，你会看到dom节点各个对象属性

3. 丰富样式输出

   - 文字样式（输出的文字内容具有css样式）

     ```javascript
     console.log('%c这是带css样式的输出文字','font-size:40px;color:red;')
     ```

   - 图片输出

     ```javascript
     console.log('%c','padding:53px 114px;background:url(xxxxxxx) no-repeat;line-height:228px;')
     ```

     ##### 严格低说，console.log并不支持直接输出图片，所以我们只能通过背景图来实现，但是背景图不能设置width和height样式，这就需要我们通过padding来撑开，还需要设置上line-height属性：

     - line-height的值取图片高度
     - background背景图相关
     - padding左右的值是图片宽的一半，上下的值大约是图片高一半再减8

#### 除了`console.log`之外的几大输出调试函数

- `console.error()`

  他输出的字体为红色，除了可以输出错误信息之外，还可以调用这个函数的一瞬间的调用栈

- `console.trace()`

  他也可以打印出调用一瞬间的调用栈，只是位置和`console.log`有所区别

- `console.time()`和`console.timeEnd()`

  ```javascript
  console.time('lll');
  xxxxxx(一段执行的js代码)
  console.timeEnd('lll');
  ```

  输出了中间代码执行时间，可以将一个字符串作为参数来区分不同的计时

- `console.count()`

  这是一个计数器，我们可以传一个名字给它，然后每次调用这个名字的时候，它就能打印这样的调用一共执行了多少次

  ```javascript
  var a = function(){
      console.count('这是a')
  }；
  var b = function(){
      a()
  };
  var c = function(){
      b()
  };
  b();
  c();
  // 这是a:1
  // 这是a:2
  ```

- `console.assert()`

  assert，也就是断言，函数有两个参数，第一个可以是一个表达式，当表达式为假时输出第二个参数的信息

  ```javascript
  console.assert(document.body,'在dom中没有body这个节点')
  ```

- `console.group()`

  分组输出，分组可以嵌套

  ```javascript
  console.group('a')
  console.log(11)
  console.log(22)
  console.group('b')
  console.log('aaa')
  console.log('bbb')
  console.groupEnd('b')
  console.groupEnd('a')
  ```

- `console.clear()`

  清除调用之前的调试输出

- .........（实在太多了，总结不完）