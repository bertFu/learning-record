# 修改Atom 隐藏.gitignore忽略的文件/文件夹的配置

假如Atom打开的文件夹有.gitignore 文件，会隐藏在.gitignore 忽略的文件/文件夹，参考链接中的设定，

在File->Setting,在Setting,点击"Open config Folder",在打开的新窗口中，可以配置config.cson,

将config.cson中的:

```
"tree-view":
hideVcsIgnoredFiles: true
```

改为

```
"tree-view":
hideVcsIgnoredFiles: false
```

也可以进入设置，选择 `tree-view`，勾选 `hideVcsIgnoredFiles` 项。
