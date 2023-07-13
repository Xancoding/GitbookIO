# Git

* [Git | AcWing Linux 基础课](https://www.acwing.com/file\_system/file/content/whole/index/content/2932078/)
* [Git 原理入门](https://www.ruanyifeng.com/blog/2018/10/git-internals.html)
* [**Learn Git Braching - Game**](https://learngitbranching.js.org/?locale=zh\_CN)  通过基于浏览器的游戏来学习 Git&#x20;
* [**Pro Git Online**](https://git-scm.com/book/zh/v2)  **强烈推荐**！学习前五章的内容可以教会您流畅使用 Git 的绝大多数技巧，因为您已经理解了 Git 的数据模型。后面的章节提供了很多有趣的高级主题。
* [**Visualizing Git Concepts with D3**](http://onlywei.github.io/explain-git-with-d3/) 借助**可视化**直观地理解一些基本的 git 概念
* [Oh Shit, Git!?!](https://ohshitgit.com/)   简短的介绍了如何从 Git 错误中恢复；
* [Git for Computer Scientists](https://eagain.net/articles/git-for-computer-scientists/)   简短的介绍了 Git 的数据模型，包含较少的伪代码以及大量的精美图片
* [Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/)  详细的介绍了 Git 的实现细节，而不仅仅局限于数据模型
* [How to explain git in simple words](https://smusamashah.github.io/blog/2017/10/14/explain-git-in-simple-words)
* [A Visual Git Reference](https://marklodato.github.io/visual-git-guide/index-en.html)

***

## Git是什么？

> **Git是一个开源的分布式版本控制系统，可以有效、高速地处理从很小到非常大的项目版本管理**

## Git常用命令

### **常用命令**

1. `git add XX`：将`XX`文件添加到暂存区
2. `git commit -m "给自己看的备注信息"`：将暂存区的内容提交到当前分支
3. `git push -u (第一次需要 -u 以后不需要)` ：将当前分支推送到远程仓库
4. `git clone git@git.acwing.com:xxx/XXX.git`：将远程仓库`XXX`下载到当前目录下
5. `git log`：查看当前分支的所有版本
6. **`git status`：查看仓库状态**
7. **有时候暂存了更改，尚未提交至仓库，如何取消暂存的更改？**
   * **`git restore --staged XX`或`git reset HEAD XX`：将`XX`从暂存区里移除**
8. **有时候修改了本地工作目录（通常也叫工作区）的文件，如何放弃本地更改（尚未暂存）？**
   * **`git checkout XX`或`git restore XX`：将`XX`文件尚未加入暂存区的修改全部撤销**

### **Git 全局设置**

1. `git config --global user.name xxx`：设置全局用户名，信息记录在`~/.gitconfig`文件中
2. `git config --global user.email xxx@xxx.com`：设置全局邮箱地址，信息记录在`~/.gitconfig`文件中
3. `git init`：将当前目录配置成`git`仓库，信息记录在隐藏的`.git`文件夹中

### **Git 查看命令**

1. `git diff XX`：查看`XX`文件相对于暂存区修改了哪些内容
2. `git status`：查看仓库状态
3. `git log`：查看当前分支的所有版本
4. `git log --pretty=oneline`：用一行来显示
5. `git reflog`：查看`HEAD`指针的移动历史（包括被回滚的版本）
6. `git branch`：查看所有分支和当前所处分支
7. `git pull` ：将远程仓库的当前分支与本地仓库的当前分支合并

### **Git 删除命令**

1. `git rm --cached XX`：将文件从仓库索引目录中删掉，不希望管理这个文件
2. `git restore --staged xx`：将`xx`从暂存区里移除
3. `git checkout — XX`或`git restore XX`：将`XX`文件尚未加入暂存区的修改全部撤销

### **Git 代码回滚**

1. `git reset --hard HEAD^`或`git reset --hard HEAD~` ：将代码库回滚到上一个版本
2. `git reset --hard HEAD^^`：往上回滚两次，以此类推
3. `git reset --hard HEAD~100`：往上回滚100个版本
4. `git reset --hard 版本号`：回滚到某一特定版本

### **Git 远程仓库**

正常的推送更改流程为：先 Add 和 Commit 本地修改，然后拉取远端更改，如果此时出现了合并冲突，解决合并冲突。然后，在合并冲突解决后推送更改。

1. `git remote add origin git@git.acwing.com:xxx/XXX.git`：将本地仓库关联到远程仓库
2. `git push -u (第一次需要-u以后不需要)` ：将当前分支推送到远程仓库
3. `git push origin branch_name`：将本地的某个分支推送到远程仓库
4. `git clone git@git.acwing.com:xxx/XXX.git`：将远程仓库XXX下载到当前目录下
5. `git push --set-upstream origin branch_name`：设置本地的`branch_name`分支对应远程仓库的`branch_name`分支
6. `git push -d origin branch_name`：删除远程仓库的`branch_name`分支
7. `git checkout -t origin/branch_name`：将远程的`branch_name`分支拉取到本地
8. `git pull`：将远程仓库的当前分支与本地仓库的当前分支合并
9. `git pull origin branch_name`：将远程仓库的`branch_name`分支与本地仓库的当前分支合并
10. `git branch --set-upstream-to=origin/branch_name1 branch_name2`：将远程的
11. `branch_name1`分支与本地的`branch_name2`分支对应

### **Git 分支命令**

1. `git branch branch_name`：创建新分支
2. `git branch`：查看所有分支和当前所处分支
3. `git checkout -b branch_name`：创建并切换到`branch_name`这个分支
4. `git checkout branch_name`：切换到`branch_name`这个分支
5. `git merge branch_name`：将分支`branch_name`合并到当前分支上
6. `git branch -d branch_name`：删除本地仓库的`branch_name`分支
7. `git push --set-upstream origin branch_name`：设置本地的`branch_name`分支对应远程仓库的`branch_name`分支
8. `git push -d origin branch_name`：删除远程仓库的`branch_name`分支
9. `git checkout -t origin/branch_name`：将远程的`branch_name`分支拉取到本地
10. `git pull` ：将远程仓库的当前分支与本地仓库的当前分支合并
11. `git pull origin branch_name`：将远程仓库的`branch_name`分支与本地仓库的当前分支合并
12. `git branch --set-upstream-to=origin/branch_name1 branch_name2`：将远程的
13. `branch_name1`分支与本地的`branch_name2`分支对应

### **Git stash 暂存**

1. `git stash`：将工作区和暂存区中尚未提交的修改存入栈中
2. `git stash apply`：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素
3. `git stash drop`：删除栈顶存储的修改
4. `git stash pop`：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素
5. `git stash list`：查看栈中所有元素

### Git更换远程仓库地址

```csharp
git remote -v  # 查看远端地址
git remote # 查看远端仓库名
git remote rm origin # 删除远程的仓库
git remote add origin https://github.com/xx/xx.git （新地址） # 重新添加远程仓库
git push --set-upstream origin master
```

### 本地项目上传到GitHub

1. 配置`ssh-key`实现本地与`Git`服务器免密交互

```
ssh-keygen  # 生成密钥
cat .ssh/id_rsa.pub
# 复制密钥，提交到 git 服务器的 ssh 密钥中
```

2. 按照下面的操作在本地文件夹配置一下`Git`：

```
git config --global user.name xxx  # 设置用户名
git config --global user.email xxx@xxx.com  # 设置用户邮箱

git init
git add .
git commit -m "xxx"
git remote add origin https://github.com/xxx/XXX.git  # 建立连接
git push -u origin master
```

### .gitignore的作用

**工程常识：缓存文件，可执行文件，编译文件 不要传到自己的 git 项目里**

**.gitignore的作用就是帮助我们在git add时将我们指定的一些文件自动排除在外，不提交到git当中**

**在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件**
