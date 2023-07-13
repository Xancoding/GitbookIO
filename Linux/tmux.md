# Tmux

* [Tmux和Vim | AcWing Linux 基础课](https://www.acwing.com/file\_system/file/content/whole/index/content/2855620/)
* [Tmux 使用教程 | 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2019/10/tmux.html)
* [Tmux 简介与使用 | Huoty's Blog](http://kuanghy.github.io/2016/09/29/tmux)

***

## Tmux 是什么？

> **`Tmux` 是一个用于在终端窗口中运行多个终端会话的工具，即终端复用软件（terminal multiplexer）**
>
> **远程 `SSH` 访问服务器进行工作时。即使非正常掉线，它能保存当前工作状态，并保证当前任务继续运行。**

## Tmux && Session && Window && Pane

* 一个`tmux` 可以有好多个`session`(会话)
* 一个`session`可以有好多个`window`(窗口)
* 一个`window`可以有好多个`pane`(面板)
* 一个`session`里不超过10个`window`是最方便的：可以用0到9迅速切换

## Tmux常用命令

> **Ac-Terminal 下前缀键被修改成了 Ctrl + a，一般的默认情况下是 Ctrl + b**

1. `tmux new -s <session-name>`：新建会话
2. `tmux detach` or `Ctrl + a d`：分离会话，退出当前 Tmux 窗口，但是会话和里面的进程仍然在后台运行
3. `tmux attach -t <session-name>`：重新接入某个已存在的会话
4. `tmux kill-session -t <session-name>`：杀死某个会话
5. `tmux switch -t <session-name>`：切换会话
6. `tmux rename-session -t <old-name> <new-name>`：重命名会话
7. `tmux ls` or `Ctrl + a s`：查看当前所有的 Tmux 会话
8. 在`tmux`中选中文本时，需要按住 `shift` 键
9. `tmux`中复制/粘贴文本：
   1. 按下 `Ctrl + a` 后松开手指，然后按 `[`
   2. 用鼠标选中文本，被选中的文本会被自动复制到tmux的剪贴板
   3. 按下 `Ctrl + a` 后松开手指，然后按 `]` ，会将剪贴板中的内容粘贴到光标处

