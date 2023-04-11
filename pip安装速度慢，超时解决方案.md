##### 解决方案：

```
找到C:\Users\Administrator\AppData\Roaming
在这个路径下新建一个名为pip的文件夹
在这个文件夹中新建一个pip.ini的配置项
编辑配置项如下：
[global]
timeout = 60000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
use-mirrors = true
mirrors = https://pypi.tuna.tsinghua.edu.cn

文档中的链接地址还可以更换其他的如下：

阿里云 http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

豆瓣(douban) http://pypi.douban.com/simple/

清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```

