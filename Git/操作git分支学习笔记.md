### 2016-03-23 操作git分支学习笔记

上次学习了git简单分支操作指令，今天玩玩看如何将创建的分支与祖分支合并。

```
git checkout develop
git branch test
git checkout test
```
首先我们进入 `develop` 分支下创建了 `test` 分支，并进入 `test` 分支。
这时候和预期的一样，在 `test` 分支下有一份 `develop` 相同的项目。

这时候我们操作 `test` 分支创建一个 `test.md` 文件，对其添加一些简单的数据 `helloworld`。

```
git checkout develop
```
我们切回 `develop` 分支，发现他同样有这个文件，里面的内容也是 `helloworld`.

```
git checkout test
git add test.md
git checkout develop

A	test.md
Switched to branch 'develop'
```
这时我们切回 `test` 分支，将 `test.md` 使用 `git add` 操作一遍，在回到 `develop`。
这时系统提示我们说 `A	test.md` 表示这个文件是处于 `add` 状态的。这个时候两边的文件都是 `add`状态。

```
git checkout test
git commit -m 'test.md'
git checkout develop
```
那我们在切换回 `test` 分支，使用 `git commit` 将文件提交到本地仓库，在切换回 `develop` 分支，神奇的发现 `test.md` 文件不见了？
我们在回去 `test` 看看，文件是否是在 `test` 分支中。

```
git checkout test
```
这是我们会发现 `test.md` 文件是存在的。也就是说，当我们在使用 `git commit` 命令时，添加到本地仓库的文件只属于 `test` 分支。
而我们回去 `develop` 分支，Git会将工作目录回复上次提交的状态，具体如何操作的善未可知。

这时候我们将刚刚提交的内容 `git push` 到远程仓库中。
```
git push origin test:test

 * [new branch]      test -> test
```
新建了一个远程仓库，并将我们所提交的内容添加到了远程仓库。我们在远程仓库中可以看到我们提交的内容。
那么现在我们来合并下两个分支。

```
git checkout develop
git merge test

Updating ae291bd..56ff540
Fast-forward
 test.md | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 test.md
```
可以看到，在 `develop` 分支下加入了新的东西 `test.md` 就是我们刚提交的内容。
那么远程分支会不会有这个新东西呢？观察后发现，远程分支是没有 `test.md` 这个文件的。

```
git status
```
查看Git的状态也没有发现 `test.md` 文件的信息。
如果本地仓库是有 `test.md` 文件的话，我们将本地仓库推入远程仓库，看看操作后远程仓库是否有该文件。

```
git push origin develop:develop
```
push后发现，远程仓库多出了一个 `test.md` 文件，简单的分支合并就完成了。
那么问题来了，如果两个分支同时操作 `test.md` 文件会发生什么事情呢？

```
git add test.md
git commit -m 'test.md'
git checkout test
git add test.md 
git commit -m 'test-test.md'

Auto-merging test.md
CONFLICT (content): Merge conflict in test.md
Automatic merge failed; fix conflicts and then commit the result.
```
我们修改两个 `test.md` 给入不同数据，提交后，先在本地合并。
系统提示，自动合并失败，两个文件有冲突，需要解决冲突后提交结果。

```
git add test.md
git commit -m 'test.md'
git push origin develop:develop
```
修改冲突后，提交到远程仓库，这个时候两个分支的数据就同步了。所以对于冲突，Git已经帮我们做好解决方案了。


>作者：Bert。

