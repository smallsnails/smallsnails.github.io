之前就简单的用用git，有些命令也没怎么使用过，今天好好整理下，方便日后自己忘了可以查看。

git使用文档地址https://gitee.com/progit/

一：下载安装git

下载地址：https://git-scm.com/downloads

安装步骤：见原文档

二：配置

配置的是你个人的用户名称和电子邮件地址，每次 Git 提交时都会引用这两条信息。
```bash
//原文示例
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

三：获取项目的git仓库

1. 没有创建git仓库的情况：

   git init 初始化创建git仓库

   git add xxx 添加要提交的文件xxx

   git commit -m 'xxx' 对添加文件的说明

2. git仓库项目已经存在的情况：
```bash
// 格式：git clone [url]  
git clone git://github.com/schacon/grit.git
```

四：更新仓库

1. 忽略某些文件提交，我们可以创建.gitignore文件，列出要忽略的文件模式。格式见原文。（最好提前设置好）

2. git add xxx 添加要提交的文件到暂存区

3. git status 检查文件状态，哪些文件修改了（如果想查看文件的哪些地方修改了，可以使用git diff；每次准备提交前，先用 git status 看下，是不是都已暂存起来了）

4. git commit 提交更新（git commit -m "xxx"添加提交说明；git commit -a跳过使用暂存区域）

```bash
// 原文示例
git commit -a -m 'added new benchmarks'
```

5. 其他一些操作 

   ①移除文件：git rm xxx（提交文件之后，仓库里面该文将不存在；如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f；如果仅暂存区域移除，使用git rm --cached xxx）

   ②移动文件：git mv file_from file_to

五：查看提交历史

git log 会按提交时间列出所有的更新，最近的更新排在最上面

git log -p -2 其中-p 选项展开显示每次提交的内容差异，用 -2 则仅显示最近的两次更新

git log --stat 显示摘要

git log --pretty 可以指定使用完全不同于默认格式的方式展示提交历史。比如用 oneline 将每个提交放在一行显示，这在提交数很大时非常有用。另外还有 short，full 和 fuller可以用，展示的信息或多或少有些不同，请自己动手实践一下看看效果如何。

还有一些其他的，见原文档。。。

六：撤销操作

git commit --amend

七：远程仓库的使用

1. 查看当前远程库

    git remote 列出远程库的名称 （至少可以看到一个名为 origin 的远程库）

    git remote -v 显示对应的克隆地址

2. 添加远程库

    git remote add [shortname] [url]，

```bash
// 其中字符串 pb 指代对应的仓库地址了
git remote add pb git://github.com/paulboone/ticgit.git
```

3. 从远程仓库抓取数据

    git fetch [remote-name] 

   有一点很重要，需要记住，fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分 支，只有当你确实准备好了，才能手工合并。

    使用 git pull 命令自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支。在日常工作中我们经常这么用，既快且好。实际上，默认情况下 git clone 命令本质上就是自动创建了本地的 master 分支用于跟踪远程仓库中的 master 分支（假设远程仓库确实有 master 分支）。所以一般我们运行 git pull，目的都是要从原始克隆的远端仓库中抓取数据后，合并到工作目录中的当前分支。

4. 推送数据到远程仓库

     git push [remote-name] [branch-name]。如果要把本地的 master 分支推送到 origin 服务器上（再次说明下，克隆操作自动使用默认的 master 和 origin 名字），可以运行下面的命令：git push origin master

5. 查看远程仓库信息

 git remote show [remote-name] ，比如要看所克隆的 origin 仓库，使用git remote show origin

6. 远程仓库的删除和重命名

 git remote rename 修改某个远程仓库在本地的简称，如：git remote rename pb pual

 git remote rm 移除对应的远端仓库，如：git remote rm pual

八：打标签

1. 列出已有标签 git tag 

2. 含附注标签 git tag -a v1.0 -m "xxx" 

3. 签署标签 git tag -s v1.0 -m "xxx"

4. 轻量级标签 git tag v1.0

5. 验证标签 git tag -v [tag-name] ，如：git tag -v v1.0

6. 后期加注标签 git tag -a [tag-name] [加注内容]

7. 分享标签 git push origin [tagname] 

九：git分支

git branch 列出所有分支

git branch -v 查看各个分支的信息

git branch --merge 查看哪些分支已被并入当前分支

git branch --no-merged 查看尚未合并的分支

git branch xxx 创建新分支 xxx

git checkout xxx 切换到新分支

git checkout -b  xxx 新建并切换到xxx分支

git merge xxx 合并分支xxx

git branch -d xxx 删除分支xxx（分支中还包含着尚未合并进来的，使用git branch -D xxx进行强制删除）

十：git命令说明：

git clone

git status

git add

git push origin master 

git commit -m 'commnet'

git checkout -b 

git checkout master 返回主分支

git merge 

git branch -D 删除本地分支

git push origin 删除线上分支

git reset --hard head^ 返回上一版本

git log或git git reflog查看提交日志

git config -- list  要检查已有的配置信息，可以使用 

git help xxx 获取xxx帮助

参考https://www.cnblogs.com/tocy/p/git-command-line-manual.html
