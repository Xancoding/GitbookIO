# 🐪 Vim

* [第 3 讲 - 编辑器 (Vim) | The missing semester of your CS education](https://missing-semester-cn.github.io/2020/editors/)
* [21世纪最强代码编辑器：NeoVim ——就是这些设置让它变成了编辑器之鬼 【附配置与插件教程】](https://www.bilibili.com/video/BV1y4411C7pE/?spm\_id\_from=333.999.0.0\&vd\_source=ae16ff6478eb15c1b87880540263910b)
* [**Vim 从入门到精通**](https://github.com/wsdjeg/vim-galore-zh\_cn)
* [**LazyVim-Keymaps**](https://www.lazyvim.org/keymaps)
* **vimtutor 是一个 Vim 安装时自带的教程 ，`vimtutor -g zh` for Chinese learner.**
* [**VimAwesome**](https://vimawesome.com/)    Vim 插件
* [**Vim Adventures**](https://vim-adventures.com/) **是一个学习使用 Vim 的游戏**
* [**Vim Tips Wiki**](http://vim.wikia.com/wiki/Vim\_Tips\_Wiki)
* [**Vim Advent Calendar**](https://vimways.org/2019/) **有很多 Vim 小技巧**
* [**Vim Golf**](http://www.vimgolf.com/) **是用 Vim 的用户界面作为程序语言的** [**code golf**](https://en.wikipedia.org/wiki/Code\_golf)
* [**Vi/Vim Stack Exchange**](https://vi.stackexchange.com/)
* [**Vim Screencasts**](http://vimcasts.org/)
* [**Practical Vim**](https://pragprog.com/titles/dnvim2/)**（书籍）**
* [Vim 中如何快速移动光标？](https://harttle.land/2015/11/07/vim-cursor.html)

***

## Vim 是什么？

> **VIM 是 Linux 系统上一款文本编辑器，它是操作 Linux 的一款利器。**

## Vim常用命令

1. 一般模式切换到编辑模式（常用）
   1. `i` : 在光标所处位置 **直接** 开始
   2. `a` : 在光标所处位置的 **下一个字符** 开始
   3. `o` : 在光标所处位置的 **下一行** 开始
   4. `r` : 取代当前光标处的字符，然后开始
   5. `[ESC]` : 退出编辑模式，回到一般模式
2. 光标移动操作
   1. `n<Space>` : **n** 为数字，光标 **向右移动**这一行的n个字符
   2. `n<Enter>` : **n** 为数字，光标 **向下移动**n行
   3. `%`: 跳转到**匹配的括号，**这个很有用
   4. 以单词为单位移动
      * `w` 移动到下一个单词的词首。
      * `b` 移动到上一个单词的词首。
      * `e` 移动到下一个单词的结尾。
   5. 以行为单位移动
      * `^` 移动到行首第一个词的首字母
      * `|` 移动到行首第一个字符
      * `$` 移动到 **行尾**
      * `0` : 移动到 **行首**
      * `G`: 光标移动到 **最后一行**
      * `gg` : 光标移动到 **第一行**
      * `:n` 或 `nG` : **n** 为数字，光标移动到 **第n行**
   6. 以屏幕为单位移动
      * `H` 移动到当前屏的首行。
      * `L` 移动到当前屏的尾行。
      * `M` 移动到当前屏的中间行。
      * `zt` 光标所在字符不动，将当前行移动到屏幕顶部，通常用来查看完整的下文，比如函数、类的定义。
      * `zz` 光标所在字符不动，将当前行移到屏幕中间。
      * `zb` 光标所在字符不动，将当前行移到屏幕底部。
      * `ctrl-f` 向下翻页，移动一整个屏幕。
      * `ctrl-b` 向上翻页，移动一整个屏幕。
      * `ctrl-e` 屏幕向下滚动一行。
      * `ctrl-y` 屏幕向上滚动一行。
   7. 文件之间移动
      * `<backspace>` 跳转到交替文件（上一个文件）。
      * `gt` 跳转到下一个标签页。
      * `gT` 跳转到上一个标签页。
      * `Ctrl+w h` 切换到左边窗格。
      * `Ctrl+w j` 切换到下边窗格。
      * `Ctrl+w k` 切换到上边窗格。
      * `Ctrl+w l` 切换到右边窗格。
      * `Ctrl+w w` 遍历切换窗格。
3. 查找、替换操作
   1. `/word` : 向 **光标之下** 寻找 第一个值为 **word** 的字符串
   2. `?word` : 向 **光标之上** 寻找 第一个值为 **word** 的字符串
   3. `n` : 重复 **前一个** 查找操作
   4. `N` : 反向 重复 **前一个** 查找操作
   5. `:n1,n2s/word1/word2/g` : **n1** 与 **n2** 为数字，在第 **n1** 行与 **n2** 行之间寻找 **word1** 这个字符串，并将该字符串 替换 为 **word2**
   6. `:1,$s/word1/word2/g` : 将全文的 **word1** 替换为 **word2**
   7. `:1,$s/word1/word2/gc` : 将全文的 **word1** 替换为 **word2**，且在替换前 **要求用户确认**
4. 文本操作（可搭配 `数字+<Enter>/<Space>、0、G、$` 等使用，达到预期组合效果）
   1. `v` : 选中文本
   2. `d` : 删除选中的文本
   3. `dd` : 删除当前行
   4. `y` : 复制选中的文本
   5. `yy` : 复制当前行
   6. `p` : 将复制的数据在光标的下一行/下一个位置 粘贴
   7. `u` : 撤销
   8. `Ctrl + r` : 取消撤销
   9. `>` : 将选中的文本整体 **向右缩进一次**
   10. `<` : 将选中的文本整体 **向左缩进一次**
5. 命令行操作
   1. `:w` : 保存
   2. `:w!` : 强制保存
   3. `:q` : 退出
   4. `:q!` : 强制退出
   5. `:wq` : 保存并退出
   6. `:set paste` : 设置成粘贴模式，取消代码自动缩进
   7. `:set nopaste` : 取消粘贴模式，开启代码自动缩进
   8. `:set nu` : 显示行号
   9. `:set nonu` : 隐藏行号
   10. `:noh`：关闭查找关键词高亮
6. `Ctrl + q` : 当`vim`卡死时，可以 **取消当前正在执行的命令**
