# Vim

* [第 3 讲 - 编辑器 (Vim) | The missing semester of your CS education](https://missing-semester-cn.github.io/2020/editors/)
* [VIM 大冒险 - Game](https://vim-adventures.com/)
* [**Vim 从入门到精通**](https://github.com/wsdjeg/vim-galore-zh\_cn)
* [**LazyVim-Keymaps**](https://www.lazyvim.org/keymaps)
* NeoVim\&LazyVim
  1. [Lazyvim配置教程](https://www.bilibili.com/video/BV1ds4y1P7Rs/?spm\_id\_from=333.337.search-card.all.click\&vd\_source=ae16ff6478eb15c1b87880540263910b)
  2. [NeoVim](https://www.bilibili.com/video/BV1zY4y1Z7FR/?vd\_source=ae16ff6478eb15c1b87880540263910b)
  3. [ChatGPT + Neovim](https://www.bilibili.com/video/BV18o4y1b7D3/?spm\_id\_from=333.999.0.0\&vd\_source=ae16ff6478eb15c1b87880540263910b)
  4. [_Configuration_](https://github.com/jackMort/ChatGPT.nvim#configuration)

***

#### Vim 是什么？

> **VIM 是 Linux 系统上一款文本编辑器，它是操作 Linux 的一款利器。**

#### Vim常用命令

1. 一般模式切换到编辑模式（常用）
   1. `i` : 在光标所处位置 **直接** 开始
   2. `a` : 在光标所处位置的 **下一个字符** 开始
   3. `o` : 在光标所处位置的 **下一行** 开始
   4. `r` : 取代当前光标处的字符，然后开始
   5. `[ESC]` : 退出编辑模式，回到一般模式
2. 光标移动操作
   1. `n<Space>` : **n** 为数字，光标 **向右移动**这一行的n个字符
   2. `n<Enter>` : **n** 为数字，光标 **向下移动**n行
   3. `0` 或 `功能键[Home]`: 光标移动到 **本行开头**
   4. `$` 或 `功能键[End]`: 光标移动到 **本行末尾**
   5. `:n` 或 `nG` : **n** 为数字，光标移动到 **第n行**
   6. `G`: 光标移动到 **最后一行**
   7. `gg` : 光标移动到 **第一行**
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
