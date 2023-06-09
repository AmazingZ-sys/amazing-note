## 函数式编程

### 什么是函数式编程

>函数式编程（Functional Programming，FP），FP是编程范式之一，我们常说的编程范式还有面向过程编程（C/C++）、面向对象编程（Java/go）

- 面向对象编程：把现实中的事物抽象成类和对象，通过封装、继承、多态等特性来演示事物和事件的联系（万物皆为对象）
- 面向过程编程
- 函数式编程：把现实世界中事物之间的联系抽象成函数（对运算过程进行抽象）
  - 程序的本质：根据输入通过某种运算获得相应的输出
  - 函数：即映射（`y = f(x)`，输入`x`通过特定运算输出`y`）
  - **函数式编程中的函数指的不是程序中的函数（方法）**，而是数学中的函数（映射关系）
  - 相同的输入始终得到相同的输出（纯函数）
  - 函数式编程哦用来描述数据（函数）之间的映射关系

```javascript
// 面向过程
let num1 = 2
let num2 = 3
let sum = num1 + num2

// 函数式编程
function add(n,m) {
  return n + m
}

// 面向对象
class Math {
  sum(n, m) {
    return n + m
  }
}
let math = new Math()
console.log(math.sum(2, 3))
```

### 为什么要学习函数式编程

- 函数式编程可以抛弃 `this`
- 打包过程可以更好利用`tree shaking`过滤无用代码
- 方便测试、方便并行处理
- 有很多库可以帮助我们进行函数式开发：`lodash`、`undersore`、`ramda`

### 函数是一等公民

> 在`JavaScript`中函数就是一个普通的对象（可以通过`new Function()`实例化构造一个新的函数），我们可以把函数存储到变量/数组中，它还可以作为另一个函数的返回值

- 函数可以存在于变量中（某些匿名函数）
- 函数可以作为参数（防抖、节流函数）
- 函数可以作为返回值（某些闭包）

#### 高阶函数

- 函数作为参数

  >函数作为参数使函数更加灵活

  ```javascript
  // forEach
  function forEach(arr, fn) {
    for(let i = 0; i < arr.length; i++) {
      fn(arr[i])
    }
  }
  // filter
  function(arr, fn){
    let results = []
    for(let i = 0; i < arr.length; i++) {
      if(fn(arr[i])){
        results.push(arr[i])
      }
    }
    return results
  }
  // exp
  let arr = [1, 2, 3]
  forEach(arr, function(item){
    console.log(item)
  })
  let filterArr = filter(arr, function(item){
    return item == 1
  })
  console.log(filterArr)
  ```

- 函数作为返回值

  ```javascript
  // exp
  function mkFn() {
    let msg = "function"
    return function() {
      console.log(msg)
    }
  }
  let fn = mkFn()
  fn()
  mkFn()()
  
  // 实现once(只执行一次的函数)
  function once(fn) {
    let flag = false
    return function() {
      if(!flag) {
        flag = true
        return fn.apply(this,arguments)
      }
    }
  }
  let num = once(function(arg){
    console.log(arg)
  })
  num(1)  // 1
  num(2)  // 无输出
  ```

#### 使用高阶函数的意义

- 抽象可以帮我们屏蔽细节，只需要关注于我们要实现的目标
- 高阶函数用来抽象通用的问题
- 代码更简洁和灵活

#### 常用的高阶函数

- forEach

  ```javascript
  // forEach
  function forEach(arr, fn) {
    for(let i = 0; i < arr.length; i++) {
      fn(arr[i])
    }
  }
  ```

- map

  ```javascript
  // map
  const map = (arr, fn) => {
    let results = []
    for(let val of arr) {
      results.push(fn(val))
    }
    return results
  }
  let arr = [1, 2, 3, 4]
  arr = map(arr, v => v * v)
  console.log(arr)
  ```

- filter

  ```javascript
  // filter
  function filter(arr, fn){
    let results = []
    for(let i = 0; i < arr.length; i++) {
      if(fn(arr[i])){
        results.push(arr[i])
      }
    }
    return results
  }
  ```

- every

  ```javascript
  // every  是否所有元素都符合条件
  const every = (arr, fn) => {
  	let result = true
  	for(let val of arr) {
  		result = fn(val)
  		if(!result) {
  			break
  		}
  	}
  	return result
  }
  let arr = [9, 12, 13]
  console.log(every(arr, v => v > 10))
  ```

- some

  ```javascript
  // some   是否有一个元素符合条件
  const every = (arr, fn) => {
  	let result = false
  	for(let val of arr) {
  		result = fn(val)
  		if(result) {
        break
  		}
  	}
  	return result
  }
  let arr = [9, 12, 13]
  console.log(every(arr, v => v > 10))
  ```

- find/findIndex

- reduce

- sort

- ......

#### 闭包

>  函数和其周围的状态（词法环境）的引用捆绑在一起形成闭包
>
> 合理使用闭包可以延长变量的生命周期
>
> 滥用闭包有可能造成内存泄漏

- 可以在另一个作用域中调用一个函数的内部函数并访问到该函数的作用域中的成员（上述例子中的`once`函数就是闭包）
- 闭包的本质：函数在执行的时候会放到一个执行栈上，当函数执行完毕之后会从执行栈上移除，**但是堆上的作用域成员因为被外部引用导致不能释放**，因此内部函数依然可以访问到外部函数的成员

```javascript
// 闭包
// exp 计算某个数的n次幂
function mkPow(n) {
  return function(num) {
    return Math.pow(num, n)  // num的n次幂
  }
}
let pow1 = mkPow(2) // 计算平方
let pow2 = mkPow(3) // 计算三次方
console.log(pow1(4))  // 4的平方
console.log(pow2(5))  // 5的三次方
```

#### 纯函数

> **相同的输入始终会得到相同的输出**，而且没有任何可观察的副作用

- 纯函数类似数学中的函数`y = f(x)`
- `loadsh`是一个纯函数的功能库，提供了对数组、数字、对象、字符串、函数等操作的方法
- 数组中的`slice`和`splice`就是纯函数和非纯函数的代表
  - `slice`：数组截取，不改变原数组，相同的输入参数得到相同的输出结果
  - `splice`：数组截取，会改变原数组，相同的输入会得到不同的输出结果
- 函数式编程不会保留计算中间的结果，所以变量时不可变的（无状态的）
- 我们可以把一个函数的执行结果交给另一个函数去处理

```javascript
// exp
let arr = [1, 2, 3, 4, 5]
console.log(arr.slice(0, 3))   // [1, 2, 3]
console.log(arr.slice(0, 3))   // [1, 2, 3]
console.log(arr.slice(0, 3))   // [1, 2, 3]

console.log(arr.splice(0, 3))  // [1, 2, 3]
console.log(arr.splice(0, 3))  // [4, 5]
console.log(arr.splice(0, 3))  // []
```

#### lodash

> `loadsh`是一个一致性、模块化、高性能的`JavaScript`实用工具库，里面好多方法都用到了纯函数的概念

##### 纯函数的好处

- 可缓存：因为纯函数对相同的输入始终有相同的输出，所以可以把纯函数的结果缓存起来，再次调用时从缓存中取值以达到性能速度兼顾的作用

  ```javascript
  // exp
  const _ = require('lodash')
  function getArea(r){
    return Math.PI * r * r
  }
  let getAreaWithMemory = _.memoize(getArea)
  console.log(getAreaWithMemory(4))
  console.log(getAreaWithMemory(4))
  console.log(getAreaWithMemory(4))
  // 多次调用时只有第一次对getArea函数进行了调用，之后都是从缓存中取得结果
  
  // 手写一个缓存函数
  function momeize(fn) {
    let obj = {}
    return function() {
      // 相同的输入arguments得到相同的输出
      let key = JSON.stringify(arguments)
      obj[key] = obj[key] || fn.apply(fn,arguments)
      return obj[key]
    }
  }
  ```

- 可测试：纯函数让单元测试更为方便

- 有利于并行处理

  - 在多线程的环境下操作并共享的内存数据可能出现意外情况
  - 纯函数不需要访问共享的内存数据，所以在并行环境下可以任意运行纯函数（Web Worker）

##### 纯函数的“副作用”

- 纯函数对于相同的输入永远会得到相同的输出，而且没有任何可观察的副作用

  ```javascript
  // 非纯函数
  let min = 18
  function checkAge(age) {
    return age >= min
  }
  
  // 纯函数（硬编码，可以通过柯里化进行解决）
  function checkAge(age) {
    let min = 18
    return age >= min
  }
  ```

  副作用让一个函数变得不纯（如上），纯函数的根据相同的输入返回相同的输出，如果函数依赖于外部的状态就无法保证输出相同，就会带来副作用

- 副作用的来源

  - 配置文件
  - 数据库
  - 用户的输入
  - ......

所有的外部交互都有可能产生副作用，副作用也使得方法通用性下降不适合扩展和可重用性，同时副作用会给程序中带来安全隐患和不稳定性，但是副作用不可能完全禁止，我们需要尽可能控制它们在可控范围内发生

#### 函数的柯里化

> 我们可以通过函数的柯里化来解决硬编码的问题

```javascript
// 上例中的checkAge函数修改为普通的纯函数
function checkAge(min, age) {
  return age >= min
}

// 通过闭包进行优化(函数柯里化)
function checkAge(min) {
  return function(age) {
    return age >= min
  }
}

// es6的箭头函数优化
let checkAge = min => (age => age >= min)

let checkAge18 = checkAge(18)
let checkAge20 = checkAge(20)
console.log(checkAge18(20))
console.log(checkAge20(20))
```

##### lodash中的柯里化函数

- `_.curry(func)`

  - 功能：创建一个函数，该函数接受一个或者多个`func`的参数，如果`func`所需要的参数都被提供则执行`func`并返回执行的结果，否则继续返回该函数并等待接受剩余参数（将一个多元函数转化为一元函数）
  - 参数：需要柯里化的函数
  - 返回值：柯里化之后的函数

  ```javascript
  // exp
  const require (lodash) //要柯里化的函数
  
  function getsum (a, b, c) {
    return a + b + c
  }
  
  //柯里化后的函数
  
  let curried = lodash (getsum) //测试
  
  curried (1, 2, 3)  
  curried (1) (2) (3)  
  curried (1, 2) (3)
  
  //柯里化案例1
  
  "".match(/ハs+/g)  //匹配空格
  
  "".match(/ハd+/g)   // 匹配数字
  
  const _ =  require ('lodash)
  
  const match = _.curry (function (reg, str){
    return str match (reg)                                         
  })
  
  const havespace = match (/\s+/g)  
  const havenumber = match (/\d+/g)
  
  const filter = _.curry (function(func, array){
    return array.filter (func)
  })
  
  const findspace = filter (havespace)
  console.log(havespace ('helloworld'))
  
  console.log(havenumber ('abc'))
  
  console.log(filter (havespace,  ['John Connor', 'John_Donne']))
  
  console.log(findspace (['John Connor', 'John Donne']))
  
  // 柯里化案例2：改造getSum函数
  const curried = _.curry(getSum)
  console.log(curried(1, 2, 3))    // 参数数量相同，返回这个函数调用之后的返回值
  console.log(curried(1)(2, 3))    // 传递部分参数返回一个函数继续调用
  console.log(curried(1, 2)(3))
  ```

##### 函数柯里化原理模拟

```
function curry(func) {
	return function curriedFn(...args) {
		if(args.length < func.length) {
			return function() {
				return curriedFn(...args.concat(Array.from(arguments)))
			}
		}
		return func(...args)
	}
}
```

##### 总结

- 柯里化可以让我们给一个函数传递较少的参数得到一个已经记住了某些固定参数的新函数
- 这是一种对函数参数的缓存
- 让函数变得更灵活，让函数的粒度更小
- 可以把多元函数转换成一元函数，可以组合使用函数产生强大的功能

#### 函数的组合

> 如果一个函数要经过多个函数处理才能得到最终结果，这个时候可以把中间过程的函数合并成一个函数
>
> 函数就像是数据的管道，函数组合就是把这些管道连接起来，让数据经过多个管道得到最终结果
>
> **函数组合默认是从右到左执行**

```javascript
// exp : 求一个数组中的最后一个元素（先反转再取第一个值）
function compose(f, g) {
	return f(g(value))
}

function reverse(array) {
  return array.reverse()
}

function first(array) {
  return array[0]
}

let last = compose(first, reverse)
console.log(last([1, 2, 3, 4]))  // 4

```

##### lodash中的组合函数

- lodash中组合函数flow() 或者 flowRight() ，他们可以组合多个函数
- `flow()` 是从左到右执行
- `flowRight()` 是从右到左执行

```javascript
// exp   将数组中的最后一个元素转换成大写（先反转再取第一个值再大写）

const _ = require('lodash')

const reverse = arr => arr.reverse()
const first = arr => arr[0]
const toUpper = s => s.toUpperCase()

const f = _.flowRight(toUpper, first, reverse)

```

##### 组合函数的原理模拟

```javascript
// 从右到左执行，
// 先将参数函数反转，
// 将前一个函数的返回值传递给下个函数调用
function compose(...args) {
  return function(value) {
    return args.reverse().reduce(function (acc, fn)  {
      return fn(acc)
    }, value)
  }
}

// 箭头函数优化
const compose = (...args) => value => args.reverse().reduce((acc, fn) => fn(acc), value)
```

##### 函数组合调试案例

```javascript
// exp 使用lodash对函数组合进行调试
// NAVER SAY DIE   转化为  never-say-die
const _ = require('lodash')
// 使用柯里化对函数进行处理
const trace = _.curry((tag, v) => {
  console.log(tag,v)
  return v
})

const split = _.curry((sep, str) => _.split(str, sep))
const join = _.curry((sep, arr) => _.join(arr, sep))
const map = _curry((fn, arr) => _.map(arr, fn)

const f = _.flowRight(join('-'),trace('map之后'), map(_.toLower), trace('split之后'), split(' '))

```

##### lodash中的FP模块

- lodash中的fp模块提供了实用的函数式编程友好的方法

- 提供了不可变**函数优先，数据为后**的方法，自动进行函数柯里化处理

  ```javascript
  // exp  如上例
  const fp = require('lodash/fp')
  const f = fp.flowRight(fp.join('-'), fp.map(fp.toLower), fp.split(' '))
  ```

#### Point Free

> 我们可以把数据处理的过程定义为与数据无关的合成运算，不需要用到代编数据的那个参数，只要把简单的运算步骤合成到一起，在使用这种模式之前我们需要定义一些辅助的基本运算函数（实现方式就是函数的组合）

- 不需要指明处理的数据
- **只需要合成运算过程**
- 需要定义一些辅助的基本运算函数

```javascript
// exp
// 把一个字符串中的首字母提取并转换成大写，使用.作为分隔符
// world wild web =====> W.W.W.
const fp = require('lodash/fp')
const f = fp.flowRight(fp.join('.'), fp.map(fp.first), fp.map(fp.toUpper), fp.split(' '))

// 优化
const f = fp.flowRight(fp.join('.'), fp.map(fp.flowRight(fp.first, fp.toUpper)), fp.split(' '))
```

#### Functor(函子)

- 函子就是一个容器：包含值和值的变形关系（这个变形关系就是函数）
- 函子可以通过普通的对象来实现，该对象具有map 方法，map 方法可以运行一个函数对值进行处理（变形关系）

```javascript
// exp 实现一个函子
class Container {
  constructor(value) {
    this._value = value
  }
  map(fn) {
    return new Container(fn(this._value))
  }
}
console.log(new Container(5).map(x => x + 1).map(x => x * x))   // Container {value: 36}

// 优化变形
class Container {
  static of(value) {
    return new Container(value)
  }
  constructor(value) {
    this._value = value
  }
  map(fn) {
    return Container.of(fn(this._value))
  }
}
console.log(Container.of(5).map(x => x + 2).map(x => x * x))   // Container {value: 49}

// 函子对象返回一个新的函子对象，通过这种方法可以实现链式调用（编程）   出现空值或者undefined时会报错
```

##### 函子的总结

- 函数式编程的运算不直接操作值，而是由函子完成
- 函子就是一个实现了map 契约的对象
- 我们可以把函子想象成一个盒子，这个盒子里封装了一个值
- 想要处理盒子中的值，我们需要给盒子的map 方法传递一个处理值的函数（纯函数），由这个函数来对值进行处理
- 最终map 方法返回一个包含新值的盒子（函子）

#### MayBe函子

```javascript
// 对空值和undefined进行处理，不会报错，多次调用map时不知道什么时候出现了null/undefined
class MayBe {
  static of(value) {
    return new MayBe(value)
  }
  constructor(value) {
    this._value = value
  }
  map(fn) {
    return this.isNothing ? MayBe.of(null) : MayBe.of(fn(this._value))
  }
  
  isNothing() {
    return this._value === null || this._value === undefined
  }
}
```

#### Either函子

- 类似于if...else...的处理
- 异常会让函数变得不纯，Either函子可以用来做异常处理

```javascript
// 对报错进行处理
class Left {
  static of(value) {
    return new Left(value)
  }
  constructor(value) {
    this._value = value
  }
  map(fn) {
    return this
  }
}

class Right {
  static of(value) {
    return new Right(value)
  }
  constructor(value) {
    this._value = value
  }
  map(fn) {
    return Right.of(fn(this._value))
  }
}

function parseJSON(str) {
  try {
    // 正确执行
    return Right.of(JSON.parse(str))
  } catch(e) {
    // 出现错误时抛出错误信息
    return Left.of({ error: e.message })
  }
}


```

#### IO函子

- 内部的_value是一个函数，这里是把函数作为值来处理
- 可以把不纯的动作存储到_value中，延迟执行这个不纯的操作（惰性执行）
- 把不纯的操作交给调用者来处理

```javascript
// IO函子
const fp = require('lodash/fp')
class IO {
  static of (x) {
    return new IO(function() {
      return x
    })
  }
  constructor(fn) {
    this._value = fn
  }
  map(fn) {
    // 把当前的value和传入的fn组合成一个新的函数
    return new IO(fp.flowRight(fn, this._value))
  }
}

// exp 调用
let r = IO.of(process).map(p => p.execPath)  // 返回一个函数
console.log(r._value()) // 执行，将不纯的操作放到当前调用时
```

#### Task异步执行

- folktale是一个标准的函数式编程库
  - 和lodash、ramda不同的是，他没有提供很多功能函数
  - 只提供了一些函数式处理的操作，如：compose、curry等，一些函子如：Task、Either、MayBe等

```javascript
// task 处理异步任务
const { task } = require('folktale/concurrency/task')
const fs = require('fs')
const { split, find } = require('lodash/fp')

function readFile(filename) {
    return task(resolve => {
        fs.readFile(filename, 'utf-8', (err, data) => {
            if (err) { resolver.reject(err) }
            resolve.resolve(data)
        })        
    })
}
readFile('package.json')
.map(split('\n'))
.map(find(x => x.includes('version')))
.run()
.listen({
    onRejected: reject => {
        console.log(reject)
    },
    onResolved: resolve => {
        console.log(resolve)
    }
})
```

#### Pointed函子

- Pointed函子是实现了of静态方法的函子
- of方法是为了避免使用new来创建对象，更深层的含义是of方法用来把值放到上下文Context（把值放到容器中，使用map来处理）

```javascript
// exp
class Container {
  static of(value) {
    return new Container(value)
  }
  constructor(value) {
    this._value = value
  }
  map(fn) {
    return Container.of(fn(this._value))
  }
}
```

#### Monad函子

- monad函子是可以变扁的Pointed函子，IO(IO(x))
- 一个函子如果具有join和of两个方法并遵守一些定律就是一个Monad

```javascript
// 解决函子嵌套的问题
// IO函子
const fp = require('lodash/fp')
const fs = require('fs')
class IO {
  static of (x) {
    return new IO(function() {
      return x
    })
  }
  constructor(fn) {
    this._value = fn
  }
  map(fn) {
    // 把当前的value和传入的fn组合成一个新的函数
    return new IO(fp.flowRight(fn, this._value))
  }
  join () {
      return this._value()
  }
  flatMap (fn) {
      return this.map(fn).join()
  }
}

let readFile = function (filename) {
    return new IO(function() {
        return fs.readFileSync(filename, 'utf-8')
    })
}
let print = function (x) {
    return new IO(function() {
        console.log(x)
        return x
    })
}

// let cat = fp.flowRight(print,redFile)
// let r = cat('package.json')._value()._value()
// console.log(r)

let r = readFile('package.json')
// .map(x => x.toUpperCase())
.map(fp.toUpper)
.flatMap(print)
.join()
console.log(r)
```

## JavaScript异步编程

### 同步模式和异步模式

- 同步模式：一个任务执行完再执行下一个任务（逐行执行（队列，先进先出，阻塞程序））
- 异步模式：不会等待任务结束之后再执行下一个任务，采用回调函数的方式进行

### 事件循环和消息队列

### 异步编程的几种方式

### Promise异步方案、宏任务/微任务队列

#### Promise特性

- Promise对象的then方法会返回一个全新的Promise对象
- 后面的then方法就是在为上一个then返回的Promise注册回调
- 前面then方法中回调函数的返回值会作为后面then方法回调的参数
- 如果回调中返回的是一个Promise，那后面的then方法的回调会等待它的结束

#### Promise的常见误区

- 嵌套使用的方式是Promise最常见的错误
- 借助Promise链式调用then方法来尽量保证异步任务的扁平化（避免回调地狱）

#### Promise链式调用

- then返回一个全新的Promise对象（每个Promise负责一个异步任务，互相之间不干扰）

  ```javascript
  const promise = new Promise((resolve, reject) => {
      resolve(100);
      // reject(new Error('promise reject'));
  })
  promise.then((e) => {
      console.log('resolve', e);
  }, (err) => {
      console.log('reject', err);
  }).then(() => {
    console.log(111)
  }).then(() => {
    console.log(222)
  }).then(() => {
    console.log(333)
  })
  ```

#### Promise的异常处理

- 回调函数中通过onreject回调进行捕获处理（只能捕获当前Promise的异常）

- 使用catch方法进行异常捕获（能捕获到链条上执行的所有Promise对象的异常）

- 手动注册全局异常事件捕获方法

  - 浏览器

    ```javascript
    window.addEventListener('unhandledrejection', event => {
        const {reason, promise} = event
    
        console.log(reason, promise)
        // reason ===> Promise 失败原因，一般是一个错误对象
        // promise ===> 出现异常的Promise对象
        event.preventDefault()
    },false)
    ```

  - node

    ```javascript
    process.on('unhandledrejection', (reason, promise) => {
        console.log(reason,promise)
        // reason ===> Promise 失败原因，一般是一个错误对象
        // promise ===> 出现异常的Promise对象 
    })
    ```

#### Promise静态方法

- resolve()：直接返回一个成功的Promise对象
  - 参数是一个字符串的话，会作为这个Promise对象返回的值进行返回
  - 参数是一个Promise对象的话，这个Promise对象会被原样返回
- reject()：直接返回一个失败的Promise对象，无论传入什么参数都作为这个Promise失败的原因进行返回

#### Promise并行执行

##### Promise.all()

> 同步执行多个Promise任务，参数传入一个含有多个Promise对象的列表，等待这些任务都执行完成之后才会进行返回，如果有一个任务出错就立刻执行Promise失败回调

##### Promise.race()

> 同步执行多个Promise任务，参数传入一个含有多个Promise对象的列表，一个Promise对象成功之后就立刻执行成功回调

#### 执行时序，宏任务/微任务

> Promise的回调会作为微任务执行，绝大多数异步调用都是作为宏任务执行

- 宏任务：回调队列中等待执行的任务
- 微任务：直接在当前任务结束过后立即执行

### Generator异步方案、Async/Await语法糖

#### Generator生成器函数

>我们先理解一下生成器的概念，用于生成可迭代对象的一般我们叫做迭代器，而生成器也可以生成可迭代对象，是一个特殊的迭代器。生成器在调用时并不会立即执行，而是通过调用内部的`next()`方法进行执行

在`JavaScript`中我们这样创建一个生成器对象

```javascript
function * foo() {
  console.log('start')
  // 使用yield关键字向外返回一个值，这个值通过调用生成器对象的next方法拿到
  // yield 关键字使函数暂停执行
  try {
    let res = yield 'foo'
  }catch(e) {
    console.log(e)
  }
}

// 生成器对象存在一个done属性，如果这个属性为true的话代表这个生成器已经执行完毕
const generator = foo()
const result = generator.next()   
console.log(result.value)      // foo   我们可以通过value属性拿到yield的值
generator.throw(new Error('xxx'))   // 我们可以通过throw方法主动抛出一个异常，然后在内部通过try...catch...进行捕获
```

#### Async/Await语法糖

>`Async...await...`其实就是一个生成器`generator`的语法糖，我们通过`*`来注册一个生成器函数，也就相当于`async + 函数名`，`await`也就相当于`generator`中的`yield`

例如上面的栗子

```javascript
async function foo() {
  console.log('start')
  try {
    let res = await 'foo'
  }catch(e) {
    console.log(e)
  }
}
```

##### promise解决了我们回调地狱的问题，而generator和async  await方案使代码更加扁平化，让我们拥有同步代码的编写方式去实现异步方案

### 手动实现一个简单的promise

```javascript
const PADDING = 'padding'   // 等待状态
const FULFILLED = 'fulfilled'    // 成功状态
const REJECTED = 'rejected'     // 失败状态

class MyPromise {
    // 实例化时传入执行器，立即执行
    constructor(executor) {
        // 捕获执行器中的错误
        try {
            executor(this.resolve, this.reject)
        } catch (e) {
            this.reject(e)
        }
    }
    // 初始化状态
    status =  PADDING
    // 成功之后的值
    value = undefined
    // 失败之后的值
    reason = undefined
    // 执行成功回调，需要能多次调用，所以存储为数组
    successCallback = []
    // 执行失败回调
    errorCallback = []
    // 成功回调
    resolve = (value) => {
        // 状态不是等待时不往下执行
        if(this.status !== PADDING) {
            return
        }
        // 修改状态为成功
        this.status = FULFILLED
        // 保存成功之后的值
        this.value = value
        // 判断成功回调是否存在，存在就调用（处理异步状态）
        // this.successCallback && this.successCallback(this.value)
        while(this.successCallback.length) {
            this.successCallback.shift()()
        }
    }
    // 失败回调
    reject = (reason) => {
        // 状态不是等待时不往下执行
        if(this.status !== PADDING) {
            return
        }
        // 修改状态为失败
        this.status = REJECTED
        // 保存失败之后的值
        this.reason = reason
        // 判断失败回调是否存在，存在就调用（处理异步状态）
        // this,errorCallback && this.errorCallback(this.reason)
        while(this.errorCallback.length) {
            this.errorCallback.shift()()
        }
    }

    then = (successCallback, errorCallback) => {
        // 有参数就传递参数，没有参数的时候返回一个函数将值传递下去
        successCallback = successCallback ? successCallback : value => value
        errorCallback = errorCallback ? errorCallback : reason => { throw reason }
        // 返回一个promise对象来实现链式调用
        let promise2 = new MyPromise((resolve, reject) => {
            // 判断状态，执行相应回调
            if(this.status == FULFILLED) {
                // 避免取不到promise2
                setTimeout(() => {
                    // 捕获错误并抛给下一个 then
                    try {
                        let x = successCallback(this.value)
                        // 判断 x 是普通值还是promise对象
                        // 如果是普通值直接调用 resolve 返回
                        // 如果是 promise 需要查看这个 promise 对象的返回结果
                        // 根据这个 promise 对象的返回结果来决定调用 resolve 还是 reject
                        resolvePromise(x, resolve, reject, promise2)
                    } catch(err) {
                        reject(err)
                    }
                },0)
            }else if(this.status == REJECTED){
                setTimeout(() => {
                    // 捕获错误并抛给下一个 then
                    try {
                        let x = errorCallback(this.reason)
                        // 判断 x 是普通值还是promise对象
                        // 如果是普通值直接调用 resolve 返回
                        // 如果是 promise 需要查看这个 promise 对象的返回结果
                        // 根据这个 promise 对象的返回结果来决定调用 resolve 还是 reject
                        resolvePromise(x, resolve, reject, promise2)
                    } catch(err) {
                        reject(err)
                    }
                },0)
            } else {
                // 处理异步执行情况，将成功回调和失败回调存储起来
                this.successCallback.push(() => {
                    // successCallback()
                    setTimeout(() => {
                        // 捕获错误并抛给下一个 then
                        try {
                            let x = successCallback(this.value)
                            // 判断 x 是普通值还是promise对象
                            // 如果是普通值直接调用 resolve 返回
                            // 如果是 promise 需要查看这个 promise 对象的返回结果
                            // 根据这个 promise 对象的返回结果来决定调用 resolve 还是 reject
                            resolvePromise(x, resolve, reject, promise2)
                        } catch(err) {
                            reject(err)
                        }
                    },0)
                })
                this.errorCallback.push(() => {
                    // errorCallback()
                    setTimeout(() => {
                        // 捕获错误并抛给下一个 then
                        try {
                            let x = errorCallback(this.reason)
                            // 判断 x 是普通值还是promise对象
                            // 如果是普通值直接调用 resolve 返回
                            // 如果是 promise 需要查看这个 promise 对象的返回结果
                            // 根据这个 promise 对象的返回结果来决定调用 resolve 还是 reject
                            resolvePromise(x, resolve, reject, promise2)
                        } catch(err) {
                            reject(err)
                        }
                    },0)
                })
            }
        })
        // then 方法返回一个 promise 对象完成链式调用
        return promise2
    }
    // 是否成功都会执行一次
    finily = (callback) => {
        this.then((value) => {
            console.log(callback(),"====>caa")
            // 成功拿到执行结果包装为 promise 对象抛出
            return MyPromise.resolve(callback()).then(() => value)
        }, (reason) => {
            // 失败拿到执行结果包装为 promise 对象抛出
            return MyPromise.resolve(callback()).then(() => { throw reason })
        })
        // this.then((value) => {
        //     callback()
        //     return value
        // }, reason => {
        //     callback()
        //     throw reason
        // })
    }
    // 捕获 promise 的执行异常然后抛出
    catch = (failCallback) => {
        return this.then(undefined, failCallback)
    }
    // 静态方法 all
    static all (arr) {
        let results = []
        let index = 0
        return new MyPromise((resolve, reject) => {
            function addData(key,value) {
                results[key] = value
                index++
                // 等待异步操作完成之后一起返回
                if(index === arr.length) {
                    resolve(results)
                }
            }
            for(let i = 0; i < arr.length; i++) {
                let current = arr[i]
                if(current instanceof MyPromise) {
                    // promise 对象
                    current.then(value => {
                        addData(i, value)
                    }, reason => {
                        reject(reason)
                    })
                }else{
                    // 普通值
                    addData(i, current)
                }
            }
        })
    }

    static resolve (value) {
        // 是 promise 对象直接返回
        console.log(value instanceof MyPromise,"=====>bool")
        if(value instanceof MyPromise) return value
        // 是普通值包装为 promise 对象再返回
        return new MyPromise(resolve => resolve(value))
    }
}

// 判断返回值是否是 promise 对象 是不是返回了自身对象
function resolvePromise(x, resolve, reject, promise2) {
    // 避免循环引用报错(自己返回自己)
    if(x === promise2) {
        return reject(new TypeError('xxxxxxx'))
    }
    if(x instanceof MyPromise) {
        // promise 对象
        // x.then(value => {resolve(value), reason => reject(reason))})
        x.then(resolve, reject)
    }else {
        // 普通值
        resolve(x)
    }
}
```



