# Shell

* [**Shell 语法 | AcWing Linux 基础课**](https://www.acwing.com/file\_system/file/content/whole/index/content/2855883/)
* [**常用命令 | AcWing Linux 基础课**](https://www.acwing.com/file\_system/file/content/whole/index/content/3030414/)
* [《Bash 脚本教程》](https://wangdoc.com/bash/)
* [第 1 讲 - 课程概览与 Shell | The missing semester of your CS education](https://missing-semester-cn.github.io/2020/course-shell/)
* [第 2 讲 - Shell 工具和脚本 | The missing semester of your CS education](https://missing-semester-cn.github.io/2020/shell-tools/)
* [# 帮你打造超酷的shell终端！zsh+oh my zsh+powerlevel10k](https://www.bilibili.com/video/BV1dX4y127JL/?spm\_id\_from=333.788.top\_right\_bar\_window\_history.content.click\&vd\_source=ae16ff6478eb15c1b87880540263910b)

***

## Shell是什么？

* `Shell` 是我们通过命令行与操作系统沟通的 `程序`，是个 `命令行解释器`
* `Shell` 脚本可以直接在命令行中执行，也可以将一套逻辑组织成一个文件，方便复用
* `Shell` 负责外界与 `Linux 内核` 的交互，接收用户或其他应用程序的命令，然后把这些命令转化成内核能理解的语言，传给内核，内核是真正干活的，干完之后再把结果返回用户或应用程序
* `Shell`解释器有 `sh`、`bash`、`zsh...`

## Shell常用命令

1. `ag xxx`：搜索当前目录下的所有文件，**检索`xxx`字符串**
2. `find /path/to/directory/ -name '*.py'`：**搜索**某个文件路径下的所有`*.py`**文件**
3. `history`：展示当前用户的历史操作。内容存放在`~/.bash_history`中
4. `grep xxx`：从`stdin`中读入若干行数据，如果某行中包含`xxx`，则输出该行；否则忽略该行，**用于查找文件里符合条件的字符串**
