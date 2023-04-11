#### `git`撤销提交到本地的`commit`操作:

在我们写完代码进行提交的时候我们常常这样做：

```
git add .    // 添加所有修改过的文件待提交
git commit -m "xxxx"    // 提交到本地仓库
git push     // 推送到远程分支
```

在我们执行过`commit`之后，想撤回`commit`，怎么办？

```
// 执行如下操作
git reset --soft HEAD^  // 撤销commit 代码改变仍然保留
// HEAD^ 表示回到上一个版本（在push之前你可能有多次commit），也可以写成DEAD~1
// 如果你进行了2次的commit 都想撤回  可以使用HEAD~2
```

`reset`之后的几个参数：

- `--mixed`：表示不删除工作控件改动过的代码，撤销`commit`，并且撤销`git add . `操作，这个为默认参数（`git reset --mixed HEAD^`和`git reset HEAD^`操作效果一样）
- `--soft`：表示不删除工作空间代码，撤销`commit`，保留`git add .`操作
- `--hard`：表示删除工作空间代码，撤销`commit`，撤销`git add .`操作，在完成这个操作之后恢复到上一次`commit`的状态（即有改动的代码没了）