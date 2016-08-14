# 快捷键

KEY                  | SYMBOL
:------------------- | :-----
alt or option on Mac | ⌥
cmd on Mac           | ⌘
ctrl                 | ^
shift                | ⇧
left                 | ←
up                   | ↑
right                | →
down                 | ↓
backspace            | ⌫

## Common Bindings

KEY (MAC)            | Header Two
:------------------- | :---------
`cmd-t`              | 快速打开最近浏览文件
`cmd-\`              | 打开关闭菜单
`ctrl-0`             | 切换到侧栏菜单
`cmd-k-<arrow>`      | 分屏
`option-shift-cmd-D` | 打开文件差异对比
`option-shift-D`     | 打开代码诊断窗口

## Development

KEY (MAC)            | COMMAND                                        | Header Two
:------------------- | :--------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------
`option-shift-cmd-D` | nuclide-diff-view:open                         | 打开文件差异对比
`option-shift-cmd-F` | nuclide-find-references:activate               | In projects such as Hack or Flow, this will allow you to find all the references to a selected, highlighted entity in your project.
`option-shift-D`     | nuclide-diagnostics-ui:toggle-table            | 打开代码诊断窗口
`option-shift-A`     | nuclide-diagnostics-ui:fix-all-in-current-file | 打开文件差异对比
`option-cmd-y`       | nuclide-hack-symbol-provider:toggle-provider   |
`cmd-shift-i`        | nuclide-format-js:format                       | 快速导入 `require` and `import` 需要的模块
`option-O`           | nuclide-outline-view:toggle                    | 快速识别方法的入参，返回值

## File Tree

KEY (MAC)         | COMMAND                      | Header Two
:---------------- | :--------------------------- | :---------
`right`           | expand-directory             | 上一个文件
`left`            | collapse-directory           | 下一个文件
`option-right`    | recursive-expand-directory   | 打开文件夹
`option-left`     | recursive-collapse-directory | 收起文件夹
`ctrl-{` `ctrl-}` | recursive-collapse-all       | 打开/收起文件夹
`backspace`       | remove                       | 删除
`cmd-\`           | toggle                       | 显示/隐藏目录树
`home`            | move-to-top                  | 到树顶部
`end`             | move-to-bottom               | 到树底部
`Enter`           | open-selected-entry          | 打开文件
`cmd-k-down`      | open-selected-entry-down     | 打开文件，在下方分屏
`cmd-k-right`     | open-selected-entry-right I  | 打开文件，在右侧分屏
`cmd-k-up`        | open-selected-entry-up       | 打开文件，在上方分屏
`cmd-k-left`      | open-selected-entry-left     | 打开文件，在左侧分屏
`cmd-shift-\`     | reveal-active-file           | 快速定位文件
`ctrl-O`          | toggle-focus                 | 切换到数

## Files

KEY (MAC)      | COMMAND                                          | Header Two
:------------- | :----------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------
`cmd-t`        | nuclide-quick-open:find-anything-via-omni-search | 查询最近打开/关闭文件
`option-cmd-o` | nuclide-open-filenames-provider:toggle-provider  | 查询最近打开文件
`option-cmd-r` | nuclide-recent-files-provider:toggle-provider    | 查询最近编辑过的关闭的文件
`option-cmd-t` | nuclide-fuzzy-filename-provider:toggle-provider  | 按模式搜索
`option-cmd-n` | nuclide-related-files:jump-to-next-related-file  | Find files related to the current file. A file is related if they have the same basename, but a different extension, for example.

## Debugger

KEY (MAC)      | COMMAND                             | Header Two
:------------- | :---------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
`option-cmd-I` | window:toggle-dev-tools             | Toggle the developer tools UI.
`shift-cmd-Y`  | nuclide-debugger:toggle             | Toggles the process attachment UI where you will choose the process on which you would like to debug (e.g., a Node process, etc.)
`f8`           | nuclide-debugger:continue-debugging | After stopping at a breakpoint, and possibly stepping throughout code, this will enable debugging to continue to the next breakpoint or end of the process.
`f9`           | nuclide-debugger:toggle-breakpoint  | If a breakpoint is set, this will unset that breakpoint; and vice-versa.
`f10`          | nuclide-debugger:step-over          | Step over a piece of code. For example, if you are stopped at a method call, this will execute that method without stepping through it line-by-line.
`f11`          | nuclide-debugger:step-into          | Step into a piece of code. For example, if you are stopped at a method call, this will go into the first line of that method.
`shift-f11`    | nuclide-debugger:step-out           | If you have stepped into a piece of code, this will step out to the point on which you entered that piece of code. For example, if you stepped into a method, this will step out back to the method call itself.
`shift-f5`     | nuclide-debugger:stop-debugging     | Stop the actual debugging process.

## Editor Panes

KEY (MAC)     | COMMAND                                      | Header Two
:------------ | :------------------------------------------- | :---------
`cmd-k-down`  | nuclide-move-pane:move-tab-to-new-pane-down  | 在下边分屏打开文件
`cmd-k-right` | nuclide-move-pane:move-tab-to-new-pane-right | 在右边分屏打开文件
`cmd-k-up`    | nuclide-move-pane:move-tab-to-new-pane-up    | 在上边分屏打开文件
`cmd-k-left`  | nuclide-move-pane:move-tab-to-new-pane-left  | 在左边分屏打开文件

## Miscellaneous

KEY (MAC)             | COMMAND                                              | Header Two
:-------------------- | :--------------------------------------------------- | :-----------------------------------------------------------------------------------------------------------------------------
`option-cmd-enter`    | hyperclick:confirm-cursor                            | When using hyperclick, this will confirm the hyperclick action you want to take.
`ctrl-option-shift-H` | nuclide-health:toggle                                | Toggle the Nuclide health tab, which show details about the Nuclide process itself (how much CPU, memory is being used, etc.).
`ctrl-option-shift-X` | nuclide-clipboard-path:copy-project-relative-path    | Copy the relative path of the current file to the clipboard.
`ctrl-shift-X`        | nuclide-clipboard-path:copy-absolute-path            | Copy the absolute path of the current file to the clipboard.
`ctrl-option-X`       | nuclide-clipboard-path:copy-repository-relative-path | Copy the relative path of the current file starting at the root of the Mercurial repository.

官方文档：（<https://nuclide.io/docs/quick-start/getting-started/）>
