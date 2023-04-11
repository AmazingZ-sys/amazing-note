#### 使用`JavaScript`遍历对象、数组

在我们的日常工作中，操作对象和数组是十分频繁的，总结一下经常使用的方法

##### 遍历对象：

1. 使用`Object.keys()`遍历

   > 返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含Symbol属性）

   ```javascript
   var aa = {"0":"a","1":"b","2":"c"};
   Object.keys(aa).forEach((key)=>{
       console.log(key,aa[key])
   })
   ```

2. 使用`for...in...`遍历

   > 循环遍历对象自身和集成的可枚举属性（不含Symbol属性）

   ```javascript
   var aa = {"0":"a","1":"b","2":"c"};
   for(var i in aa){
       console.log(i,aa[i])
   }
   ```

3. 使用`Object.getOwnPropertyName(obj)`遍历

   > 返回一个数组，包含对象自身的所有属性（不含Symbol属性，但是包括不可枚举属性）

   ```javascript
   var aa = {"0":"a","1":"b","2":"c"};
   Object.getOwnPropertyName(aa).forEach((key)=>{
       console.log(key,aa[key])
   })
   ```

4. 使用`Reflect.ownKeys(obj)`遍历

   > 返回一个数组，包含对象自身的所有属性，不管属性名是Symbol还是字符串，也不管是否可枚举

   ```javascript
   var aa = {"0":"a","1":"b","2":"c"};
   Reflect.ownKeys(aa).forEach((key)=>{
       console.log(key,aa[key])
   })
   ```

##### 遍历数组

1. 使用`forEach`遍历

   ```javascript
   var aa = [1,2,3,4];
   aa.forEach((val,index)=>{
       console.log(val,index)
   })
   ```

2. 使用`for...in...`遍历

   ```javascript
   var aa = [1,2,3,4];
   for(var i in aa){
       console.log(i,aa[i])
   }
   ```

3. 使用`for...of...`遍历

   ```javascript
   var aa = [1,2,3,4];
   for(var val of aa){
       console.log(val)
   }
   ```

   