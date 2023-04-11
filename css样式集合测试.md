##### css样式集合测试：

```javascript
地址：https://qishaoxuan.github.io/css_tricks/createTriangle/
```

##### 当前窗口是否是我打开了的那个页面：

```javascript
<script>
        var hiddenProperty = 'hidden' in document ? 'hidden' :    
        'webkitHidden' in document ? 'webkitHidden' :    
        'mozHidden' in document ? 'mozHidden' :    
        null;
    var visibilityChangeEvent = hiddenProperty.replace(/hidden/i, 'visibilitychange');
    var onVisibilityChange = function(){
        if (!document[hiddenProperty]) {   
            // document.title='Calamus_我应在江湖悠悠，饮一壶浊酒'; 
            document.title="Calamus_我应在江湖悠悠，饮一壶浊酒。"; 
            var link = document.head.querySelector("link#cl-ico")                           
            link.href="https://cdn.calamus.xyz/blog_ali/favicon.ico"
            var linkicon = document.head.querySelectorAll("link[rel=icon]")                           
            $.each(linkicon,function(i,n){
                n.href = "https://cdn.calamus.xyz/blog_ali/favicon.ico"
            })
        }else{
            document.title='你还要无视我多久o(╥﹏╥)o';
             var link = document.head.querySelector("link#cl-ico")
            link.href="https://cdn.calamus.xyz/blog_ali/favicon_gray.ico";
            var linkicon = document.head.querySelectorAll("link[rel=icon]")  
            $.each(linkicon,function(i,n){
                n.href = "https://cdn.calamus.xyz/blog_ali/favicon_gray.ico"
            })
        }
    }
    document.addEventListener(visibilityChangeEvent, onVisibilityChange);
    
    </script>
```

