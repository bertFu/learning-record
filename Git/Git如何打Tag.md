# Git tag

Git 中的tag指向一次commit的id，通常用来给开发分支做一个标记，如标记一个版本号。

git跟其它版本控制系统一样，可以打标签(tag)作用是标记一个点为一个版本号，如0.1.3, v0.1.7, ver_0.1.3.在程序开发到一个阶段后，我们需要打个标签，发布一个版本，标记的作用显而易见。<br>
下面介绍一下打标签，分享标签，移除标签的操作命令。

```shell
git tag -a 0.1.3 -m "Release version 0.1.3"
```

详解：`git tag` 是命令 -a 0.1.3是增加 名为0.1.3的标签 -m 后面跟着的是标签的注释 打标签的操作发生在我们commit修改到本地仓库之后。

完整的例子:

```shell
 git add . git commit -m "fixed some bugs"
 git tag -a 0.1.3 -m "Release version 0.1.3″
```

分享提交标签到远程服务器上

```shell
 git push origin master git push origin --tags
```

–tags参数表示提交所有tag至服务器端，普通的 `git push origin master` 操作不会推送标签到服务器端。

删除标签的命令：

```shell
git tag -d 0.1.3
```

删除远端服务器的标签：

```
git push origin :refs/tags/0.1.3
```

Tag就是当前分支引申出去的一个指针，指向单签提交的代码，可以看成是一个分支的状态，我们可以通过获取这个Tag，来获取我们需要的代码状态。

分支branch是可以做代码修改的，标签Tag是不能做修改的。
