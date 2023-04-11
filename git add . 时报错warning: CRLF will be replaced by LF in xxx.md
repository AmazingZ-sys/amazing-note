## 前言

今天，普通平凡的一天，平凡的使用 `git add .`，然后又出现一个之前没遇到的错误提示 ，如图所示：

![](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfg30v907ij31nm08wjso.jpg)

## 解决办法：

一个命令修改`git`的默认配置：

```
git config core.autocrlf false  // 将设置中自动转换功能关闭
```

### 问题解释和分析

> warning: CRLF will be replaced by LF in XXX . The file will have its original line endings in your working directory.

首先问题出在不同操作系统所使用的换行符是不一样的，下面罗列一下三大主流操作系统的换行符：

`Uinx/Linux`采用换行符`LF`表示下一行（`LF：LineFeed`，中文意思是换行）；

`Dos`和`Windows`采用回车+换行CRLF表示下一行（`CRLF：CarriageReturn LineFeed`，中文意思是回车换行）；

`Mac OS`采用回车`CR`表示下一行（`CR：CarriageReturn`，中文意思是回车）

因为文件中存在两种环境下的换行符，`git`会自动替换`CRLF`为`LF`，所以提示警告