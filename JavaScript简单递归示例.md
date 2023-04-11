```JavaScript
// 简单递归示例
var arr = [];
                function queryList(json,arr){
                    for (let i=0;i<json.length;i++){
                        var childPaths = json[i].childPaths || [];
                        if (childPaths.length==0){
                            arr.push({id:json[i].id,name:json[i].name,check:json[i].check})
                        } else {
                            var p = {id:json[i].id,name:json[i].name,check:json[i].check,childPaths:childPaths};
                            arr.push(p);
                            queryList(childPaths,arr);
                        }
                    }
                    return arr
                }
                queryList(this.pcData,arr);
```

