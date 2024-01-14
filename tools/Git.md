# Git

## Git初始化

### Git配置相关

查看git版本：`git --version`

设置用户名和邮箱：`git config --global user.name(email) <username>/<email>`

查看用户名和邮箱：`git config --global user.name(email)`

删除用户名和邮箱配置：`git config --unset --global user.name(email)`

设置git别名：`sudo git config --system alias.<alias> <Formerly>`

开启颜色显示：`git config --global color.ui true `

### Git应用

初始化命令：`git init  /  git init <filename>`

添加文件：`git add <filename> / git add .`

提交：`git commit -m "<description>"` **注意：Git要求提交说明不能为空**

在没有对工作区文件进行任何修改的情况下进行空白提交：`git commit --allow-empty -m "<description>"`



### Git信息操作

显示当前Git仓库信息：`git status`

跟踪status命令时磁盘访问：`strace -e 'trace-file' git status`

显示.git目录的位置：`git rev-parse --git-dir`

显示工作区根目录：`git rev-parse --show-toplevel`

显示相对于工作区根目录的相对目录：`git rev-parse --show-prefix`

查看git的配置文件：`git config -e <null>/--<global>/<system>`

查看仓库信息：`git log --pretty=fuller`

修正提交信息：`git commit --amend --allow-empty --reset-author` ——当没有配置用户名或者又像是author一列就会是一个猜测值“可能是当前系统名称”，在设置用户信息后通过这个来修正提交信息



## 暂存区

显示文件差异（*工作区和暂存区比较*）：`git diff`

显示文件与当前工作分支的差异（*工作区和HEAD比较*）：`git diff HEAD`

查看暂存区和版本库之间文件的差异（*暂存区和HEAD比较*）：`git diff --staged`

显示有修改的文件(精简)：`git status <-s>`

显示暂存区的目录树：`git ls-files -s`

**注意：在每次对文件修改后需要进行add操作**

撤销工作区中尚未提交的文件：`git checkout -- <filename>`

清空当前工作区非跟踪文件和目录：`git clean -fd`

用暂存区刷新工作区：`git checkout`

将暂存区目录树写入对象库：`git write-tree`,然后再执行：`git ls-tree -l <几位序列号>`

递归显示内容：`git write-tree | xargs git ls-tree -l -r -t`

保存当前工作进度：`git stash`



暂存区的示意图：

<img src="C:\Users\zhuxuanyu\Desktop\CPR\Typora\picture\image-20221213014527193.png" alt="image-20221213014527193" style="zoom: 33%;" />



## Git对象

查看不同ID的类型/内容：`git cat-file -t/-p <ID>`

<img src="C:\Users\zhuxuanyu\Desktop\CPR\Typora\picture\image-20221213135213350.png" alt="image-20221213135213350" style="zoom:33%;" />

显示当前分支：`git branch`

版本库：

<img src="C:\Users\zhuxuanyu\Desktop\CPR\Typora\picture\image-20221213140004791.png" alt="image-20221213140004791" style="zoom:50%;" />

显示引用对应的提交ID：`git rev-parse <master>/<HEAD>/<refs/heads/master>`

## Git重置

显示更简短的提交信息：`git log --graph --oneline`

**HEAD^  代表 HEAD的父提交**

refs/heads/master作为一个游标指向当前版本的最新提交，但是可以去更改它，让它指向指定的提交ID

改变游标指向：`git reset --hard <ID>`

查看游标变动记录：

1.`tail .git/logs/refs/heads/master`

2.`git reflog show master | head` ——最新改变放在了最前面，更加优化，而且还有<refname>@{number} ,可通过`git reset --hard master@{number}`直接让master指向对应的提交ID

用版本库的文件覆盖暂存区的文件：`git reset HEAD`



### git reset详解

1. `git reset`:仅用HEAD指向的目录树重置暂存区，工作区不受影响——相当于撤销之前的git add

2. `git reset HEAD`:同上

3. `git reset -- <filename>`:仅将文件filename撤出暂存区，相当于`git add <filename>`的反操作

4. `git reset HEAD <filename>` :同上

5. `git reset --soft HEAD^`:工作区和暂存区不变，回退最新一次提交——相当于`git commit --amend`的第一步操作

6. `git reset HEAD^`:工作区不变，但是暂存区会回退到上一次提交之前，引用也会回退
7. `git reset --mixed HEAD^`:同上
8. `git reset --hard HEAD^`:彻底撤销最近一次提交，引用回退到前一次提交，而且工作区和暂存区都会回退到上一次提交的状态——自上一次提交以来的提交全部丢失



## Git检出

分离头指针：指HEAD头指针指向一个具体的提交ID,而不是一个引用——因为在执行`git checkout <ID>^`前，执行`cat .git/HEAD`看到的会是`ref: refs/heads/master`表示HEAD指向master分支

查看提交ID：`git rev-parse <HEAD>/<master>`

git checkout 与git reset不同：HEAD执行master,而reset会改变master指向，checkout却只会改变HEAD,master不变

### git checkout 详解

1. `git checkout branch`:检出branch分支——更新HEAD指向branch分支，用branch分支指向的树更新暂存区和工作区
2. `git checkout`:汇总显示工作区、暂存区和HEAD的差异
3. `git checkout HEAD`:同上
4. `git checkout -- filename`:用暂存区的filename覆盖工作区的filename——相当于撤销之前的git add
5. `git checkout branch -- filename`:维持HEAD指向不变，用branch指向的分支中的filename覆盖暂存区和工作区中的filename
6. `git checkout .`**(dangers)**:取消所有本地修改，用暂存区文件直接覆盖本地文件



## 恢复进度

检测将要被删除的文件：`git clean -nd`

真正删除文件和目录：`git clean -fd`

### git stash用法详解

1. `git stash`：保存当前工作进度，分别对暂存区和工作区进行保存

2. `git stash list`：显示进度列表，即可以多次保存，回复时可以选择性恢复
3. git stash pop [--index] [<stash>]：不实用参数即恢复最新保存的工作进度，并将恢复的工作进度从存储的工作进度列表中清除；如果提供<stash>参数,则恢复从该stash中恢复；选项index除了恢复工作区的文件外还尝试恢复暂存区
4. `git stash [save [--patch]  [-k|--[no-]keep-index] [-q|--quiet] [<msg>]] `：保存工作进度的完整语法：参数[--patch]会显示工作区和HEAD的差异；-k或者keep-index参数在保存进度后不会将暂存区重置——默认将暂存区和工作区重置
5. `git stash apply [--index] [<stash>]`：不删除恢复的进度，同pop
6. `git stash drop [<stash>]`：删除一个进度，默认删除最新的进度
7. `git stash clear`：删除所有存储进度
8. `git stash branch <branchname> <stash>`：基于进度创建分支



查看git-stash脚本安装位置：`git --exec-path`

## Git 基本操作

### 合影

打标签：`git tag -m "<msg>" <tagName>`

查看标签目录：`ls .git/refs/tags/old_practices`

最近一次标签：`git describe`

### 删除

1. 不要使用rm 直接本地删除
2. `git rm <filename>`
3. `git add -u`

### 版本表示法：`git rev-parse`

### 版本范围表示法：`git rev-list`

### 浏览日志：`git log`

分支图：`git glog --oneline`

### 差异比较：`git diff`

### 文件追溯：`git blame`

### 二分查找：`git bisect`

