## 集中式版本控制与分布式版本控制

* 集中式版本控制：SVN,CVS等

* 分布式版本控制：git（简洁高效）


## git安装（略）

## git设置
```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
# git config命令的--global参数，用了这个参数，
#     表示你这台机器上所有的Git仓库都会使用这个配置，
#     但是也可以对某个仓库指定不同的用户名和Email地址。

```

## git的基本用法

1. git diff

```bash
git diff
# 这个命令可以展示与上一次提交版本不同的地方
# 是git status的细化。
```

2. git log

```bash
git log
# 这个命令可以查看提交的日志，用于在版本之间穿梭
#     这让我意识到了不能这么草率地写每次提交的摘要了！

git log --pretty=oneline
# 加了这个参数之后每次commit都会在一行中展示，
#     方便阅读和使用pipeline抽取重要日志信息。
```

3. git reset
```bash
git reset --hard HEAD
# 没什么用，仍然停留在最后提交的那个版本。
#     关键在于HEAD指针，这是一个单链表的表头，逆序可以回到最初的版本！

git reset --hard HEAD^
# 可以回到上一个版本

git reset --hard HEAD-10
# 可以回到离最近提交最近的第10个版本
```

4. git add


```bash
git add .
# 将工作区（working directory）对比最新提交的版本做的所有改动
#     添加到暂存区（stage）。
```

5. git commit

```bash
git commit -m "commit abstract"
# 将暂存区（stage）更改的内容添加到当前分支（HEAD指针指向）。
```

6. ==git checkout==

这个命令非常重要，非常有用！即可以切换目录，
也可以丢弃工作区（working directory）的修改。

```bash
git checkout -- file
# 将file在工作区的修改丢弃，回溯到git commit（repository）
#     或者git add（stage）中记录的状态。
# 但是这个也不是那么常用！

git checkout $branch_name
# 将转移到$branch_name这个branch下。

git checkout -b dev
# 创建dev分支并转移到dev分支
```

分支管理其实就是指针，多个分支就是多个指针，版本控制其实是个单链表。
你的repo在哪，取决于HEAD指针指向哪个分支。


7. git push

```bash
git push -u origin master
# 这个命令将本地的repo推送到远程repo，-u参数可以将两个repo链接起来，
#     这样以后再更新远程repo就简单了。
```

8. git merge
```bash
git merge dev
# 将dev分支合并到当前分支上。
# Git鼓励使用分支完成某个任务，合并后再删掉分支！

git branch -d dev
# 删除dev分支
```
这就是一个完整的git开发流程！


9. .gitignore

配置这个文件可以让git守护进程忽略一些格式的文件，
比如".pyc"文件等，这就很方便了！

---

## github的入门级应用

#### Step 1

在github上创建一个repsitroy。

#### Step 2
将本地的一个源代码文件夹作为repository的root directory，
并将本地已构建的代码上传到github上的repository中，实现两者的同步。

```bash
git init
git add .
git commit -m "first commit"
git remote add origin $github_repository
# github_repository表示github上的repository的地址.  
#     如https://github.com/igoingdown/sentiment-analysis.git
git push -u origin master
# 如果在多人协作的情况下可能需要更改分支！
#     主分支只能用merge来处理。
```

#### Step3
以后每次在本地更新之后，使用下面的命令来实现远程repo和本地repo的同步
```shell
bash ~/PycharmProjects/python_demo_and_tool/auto_commit_need_commit_args.sh "commit annotation"
```

---


## 实际的合作过程（为开源社区做贡献的过程）

#### step1：fork开源项目

#### step2：将本地项目更新到remote repo的最新版

* 第一次
    ```bash
    cd <your_local_dir>
    git clone <remote_repo.git>
    ```
* 除第一次以外
    ```bash
    git pull origin master
    ```


#### step3：创建dev分支
```bash
git checkout -b dev
```

#### step4：在dev上做你想做的任何修改

#### step5：提交你做的修改
```bash
git add .
git commit -m <commit message>

```

#### step6: 切换到主分支并将主分支更新到最新版

```bash
git checkout master
git pull origin master
```
==这里还不是很明白，等有问题再说吧==


#### step7: 将dev分支合并到主分支,删除dev分支，并上传主分支的更新。
```bash
git merge dev
git branch -d dev
git push
```

#### step8: 在远程repo上提交pull request


---

##发现一个讲的比较好的git教程
[我是链接](http://backlogtool.com/git-guide/cn/stepup/stepup2_5.html)


