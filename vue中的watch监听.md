##### 在以前使用vue的项目中，也用到过watch监听，因为长时间不用了，导致现在都忘记了。现在突然有了这个需求，重新看了下文档，并整理一下，详情请看官方文档：[vue官方文档watch的使用](https://cn.vuejs.org/v2/api/#watch)

> ！！！注意，不应该使用箭头函数来定义watcher函数（例如`searchQuery:newValue => this.updateAutocomplete(newValue)`）。理由是箭头函数绑定了父级作用域的上下文，所以`this` 将不会按照期望指向Vue实例，`this.updateAutocomplete` 将是undefined

- 代码示例

  ```javascript
  // watch能监听
  var vue = new Vue({
      data:{
          a:1,
          b:[],
          c:{
              d:2,
              e:"3"
          }
      },
      methods:{
          clickMethod(){
              this.c.f=4      // 通过点方法给对象添加属性，这时候watch监听不到变化
              this.$set(this.c,"f",4)   // 通过$set方法添加属性，watch能监听到变化
          }
      },
      watch:{
          // 当a的值发生改变时触发
          a:function(val,oldVal){
              console.log('new:%s,old:%s',val,oldVal)
          }，
          // 数组长度发生变化时触发
          b:function(val,oldVal){
      		console.log('new:%s,old:%s',val,oldVal)
  		}，
          // 对象中的属性值发生变化时触发
          c:{
              handler: function (val, oldVal) {
                  console.log('new:%s,old:%s',val,oldVal)
              },
        		deep: true   // 深度监听，监听到更深层级值的变化
          }
      }
  })
  ```

  