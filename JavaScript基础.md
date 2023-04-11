# JavaScript基础

## `JavaScript`的数据类型

原始数据类型：

- `boolean`：`true/false`

  关于`boolean`的数据类型转换：

  | 数据类型  |        转换为true的值        | 转换为false的值 |
  | :-------: | :--------------------------: | :-------------: |
  |  boolean  |             true             |      false      |
  |  string   |        任何非空字符串        | “”（空字符串）  |
  |  number   |         任何非零数字         |     0和NaN      |
  |  object   |           任何对象           |      null       |
  | undefined | N/A（not applicable 不适用） |    undefined    |

- `null`：空对象指针

  > 注意：`typeof null`的结果为`object`

- `undefined`：未定义的值（对变量进行了生命却没有进行赋值操作）

- `number`：数值型，对于那些极大或者极小的数，可以用`e`来表示法（即科学计数法）表示的浮点数值表示，例如：`3.125e7表示3.125乘十的7次方`

  > 由于内存的限制，`ECMAScript`并不能保存世界上所有的数值，最小数值存在`Number.MIN_VALUE`中---在大多数浏览器中，这个值为`5e-324`；最大数值存在`Number.MAX_VALUE`中---在大多数浏览器中，这个值为`1.7976931348623157e308`，如果某次计算中超过了这个范围，将会自动转换成特殊的`Infinity`值（无穷大），如果是负数则为`-Infinity`

  - 整数

  - 浮点数（小数）

  - NaN：非数值，这是一个特殊的数值，用这个数值来表示一个本来要返回数值的操作数未返回数值的情况（避免抛出错误）

    > `NaN`本身有两个非同寻常的特点：
    >
    > 1.任何涉及`NaN`的操作都会返回`NaN`
    >
    > 2.`NaN`与任何值都不相等，包括他本身

  `Number()`函数的转换规则：

  - 如果是`boolean`：`true和false`分别被转换成`0和1`
  - 如果是数字：只是简单的传入和返回
  - 如果是`null`：返回0
  - 如果是`undefined`：返回`NaN`
  - 如果是`string`，遵循以下规则：
    - 字符串中只包含数字（包括正号和负号），转换成10进制数，忽略前导零
    - 字符串中包含有效的浮点格式，转换成浮点数，忽略前导零
    - 字符串中包含有效的十六进制格式（`0xf`），转换成相同大小的十进制数
    - 空字符串转换成0
    - 字符串中包含上述以外的字符，转换成`NaN`
  - 如果是对象，则调用对象的`valueof()`方法，然后依照前面的转换规则返回的值，如果返回`NaN`，则调用对象的`toString()`方法，然后再次依照前面的转换规则进行返回

- `string`：以单引号`''`或者双引号`""`括住的

  `String()`函数遵循以下转换规则：

  - 如果值有`toString()`方法，则调用该方法并返回结果
  - 如果值是`null`，则返回`"null"`
  - 如果值是`undefined`，则返回`"undefined"`

- `symbol`

引用数据类型

- `object`：对象，即一组数据和功能的集合

  每个`object`的实例都具有以下几个属性和方法：

  - `constructor()`：保存着用于创建当前对象的函数
  - `hasOwnProperty(propertyName)`：用于检查给定属性是否在当前对象实例中（而不是在实例的原型中）
  - `isPrototypeOf(object)`：用于检查传入的对象是否是当前对象的原型
  - `propertyIsEnumerable(propertyName)`：用于检查给定的属性是否能够使用`for...in...`语句来枚举
  - `toLocaleString()`：返回对象的字符串表示，该字符串与执行环境相对应
  - `toString()`：返回对象的字符串表示
  - `valueOf()`：返回对象的字符串、数值、布尔值表示

### 面试遇见的几个问题

`null`是对象吗？为什么？

结论：`null`不是对象

解释：虽然`typeof null`会输出`object`，但这只是`JavaScript`存在的一个历史悠久的Bug。在最初版本的`JavaScript`中使用的是32位系统，为了性能考虑使用地位存储变量的类型信息，000开头表示是对象而`null`表示全为0，所以错误地判断为`object`

`"1".toString()`为什么可以调用？

解释：其实在这个语句执行的过程中做了这样的几件事情：

```javascript
var s = new String();   // 创建String实例
s.toString();   // 调用实例方法
s = null;    // 执行完毕之后立即销毁实例
```

这一整个过程体现了`基本包装类型`的性质，而基本包装类型恰恰属于基本数据类型，包括`boolean`、`number`和`string`

0.1+0.2为什么不等于0.3？

解释：0.1和0.2在转换成二进制后会无限循环，由于标准位数的限制后面多余的位数将会被截取掉，此时就已经出现了精度偏差，相加后因为浮点数小数位的限制而截断的二进制数字在转换为十进制就会变成0.30000000000000004

手动实现一个`instanceof`的功能：

核心：基于原型链的向上查找

```javascript
function myInstanceof(left, right) {
    //基本数据类型直接返回false
    if(typeof left !== 'object' || left === null) return false;
    //getProtypeOf是Object对象自带的一个方法，能够拿到参数的原型对象
    let proto = Object.getPrototypeOf(left);
    while(true) {
        //查找到尽头，还没找到
        if(proto == null) return false;
        //找到相同的原型对象
        if(proto == right.prototype) return true;
        proto = Object.getPrototypeof(proto);
    }
}
```

`JavaScript`中的`with`语句

> `with`语句的作用是将代码的作用域设置到一个特定的对象中，语法如下：
>
> `with (特定对象) statement;`
>
> 严格模式下不允许使用`with`语句，否则将会视为语法错误

定义`with`语句的目的主要是为了简化多次编写同一个对象的工作，例如：

```javascript
var ss = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;
// 可以使用 with 改写为
with(location){
  var ss = search.substring(1);
  var hostName = hostname;
  var url = href;
}
```

