- 前言
- git介绍
- 名词解析
- 常用命令
- 总结



## 前言

`Git`是程序员写项目必备的一个工具，工欲善其事必先利其器，显然这把武器对于我来时只是发挥了它最简单的功能，这不，前两天我就在分支上合错代码了，虽然同事没说什么，但是这还是很不应该犯的错，于是我决定对`Git`做一个较为详细的学习和总结。



## Git介绍

`Git`是一个开源的分布式版本控制系统，可以有效、高速的管理从很小到非常大的项目版本管理。它不需要网络的支持，现已成为很多开发人员对项目版本管理的首选，也可以用来管理自己的普通文档版本。



`Git`的命令很多，在日常开发中使用都只需要用到几个简单的命令即可，所以本文会先从_名词解析_介绍，在介绍`Git`的_常用命令_在项目中的应用。



## 名词解析

对于`Git`可以在官网下载使用，在介绍命令前也来理解几个名词概念

### 工作区

指的是在电脑里能看到的目录，目录里有你操作的各种文件



### 版本库

在工作区有一个隐藏的文件叫`.git`，这个就是`Git`的版本库，版库里包含称为`stage`的暂存库，还有`Git`给我们创建的第一个分支`master`，以及指向`master`的指针`HEDE`



### 远程仓库

在`github`、`gitlab`或`gitee`这写平台上创建的代码仓库，与本地版本库做关联，就能实现上传版本项目或文档





## 常用命令

`Git`在工作中常用的命令有

- `git config`
- `git init `
- `git clone`
- `git status`
- `git add`
- `git commit `
- `git log`
- `git reset`
- `git push `
- `git pull`
- `git stash`
- `git branch`
- `git checkout`
- `git merge`
- `git fetch`
- `git tag`

接下来会一一介绍这么命令的用法



### git config

> 配置个人信息，包括用户名和邮箱

```
git config --global user.name "my name"
git config --global user.email "my email.com"
```

这样在代码后提交的提交记录，就会是有自己设置的个人信息，一部分公司也会根据代码提交的信息对代码做审核决定年终考评奖金等



```
ssh-keygen -t rsa -C "my email.com"
```

创建秘钥，生成的秘钥放在`~/.ssh`目录下，根据提示全部回车即可，操作完成之后将公钥放在`github`仓库中，之后操作项目就免密输入了



### git init 

> 初始化git仓库

```
git init
```

初始化一个`Git`仓库



### git clone

> 克隆远程仓库

```
git clone <远程仓库链接>
```

当前的文件夹里就会把远程仓库的项目下载到本地



### git status

> 查看文件变动状态

```
git status
```

可以看到项目中的文件是否提交



### git add 

> 把工作区的文件提交到暂存区

```
git add <文件名>
git add .    // 提交工作区全部文件
```

是提交文件的第一步



### git commit

> 把工作区的文件提交到git 仓库

```
git commit -m <提交信息>
```

提交文件的第二步

```
git commit --amend -m <新的提交信息>
```

参数`--amend`不但可以修改提交信息，如果暂存区有修改的话还能把上一次的提交替换掉



**提交信息规范**

- `fix`：修复了一个` bug`
- `feat`：新增了一个功能
- `refactor`：重构某块代码
- `perf`：改进性能
- `docs`：文档相关
- `test`：测试相关
- `ci`：`CI/CD` 相关
- `chore`：其他类型

举例：如果新增了一个登陆功能提交信息可以为，_feat: 添加登陆功能_


### git 分支名
主分支	master	主分支，所有提供给用户使用的正式版本，都在这个主分支上发布
开发主分支	dev	开发分支，永远是功能最新最全的分支
功能分支	feature-*	新功能分支，某个功能点正在开发阶段
发布版本	release-*	发布定期要上线的功能
修复发布版本分支	bugfix-release-*	修复测试bug
紧急修复分支	bugfix-master-*	紧急修复线上代码的 bug


### git log

> 查看版本提交记录

```
git log  //查看最近三次提交
git log --pretty=oneline  //一行输出提交信息
```

可以查看到版本的提交信息，包括提交人，日期，提交原因，提交的唯一编号等



### git reset 

> 版本回退

```
git reset --hard HEAD^
```

`HEAD^`回退到上一个版本并且提交内容也会还原到上一个版本，`HEAD`代表版本，回退到前`n`个版本可用`HEAD~N`表示



### git push

> 把git仓库里的文件推送到远程仓库

在提交前可以给远程仓库起一个别名

```
git remote -v  //查看所有远程仓库别名
git remote add [别名] [远程仓库地址]
```

```
git push -u [别名] [分支名]
```

提交文件最后一步，`-u`用于第一次推送，可以把本地分支和远程仓库分支做关联。之后提交就不用加-u参数了。



### git pull

> 把远程仓库的最新代码拉取到本地并合并

```
git pull [别名] [分支名]
```

可以拉取远程仓库的代码并合并到本地 相当于`git fetch` 和 `git merge`



### git fetch 

> 只是拉取远程仓库代码

```
git fetch [别名] [分支名]
```

取回更新后，会返回一个`FETCH_HEAD` ，指的是某个`branch`在服务器上的最新状态，我们可以在本地通过`git log -p FETCH_HEAD`查看刚取回的更新信息

通过这些信息来判断是否产生冲突，以确定是否将更新`merge`到当前分支



### git merge 

> 将指定分支合代码并到当前分支

```
git merge <指定分支名>
```

合并代码时可能会产生冲突，解决即可正常运行



### git stash

> 保存当前修改

```
git stash
```

把当前修改版本保存下来，可以切换到其他分支或拉取远程分支也不会有冲突

```
git stash pop 
```

把保存的版本取出来

```
git stash list
```

查看所有保存版本的信息，存储可以直接通过索引的位置来获得`stash@{n}`.



### git branch

> 对分支进行增删改查的操作

```
git branch
```

查看本地分支列表

```
git branch <分支名>

-a 查看远程和本地所有分支
```

新建一个分支并命名

```
git branch -m <旧分支名> <新分支名>
```

修改本地分支名

```
git branch -d <分支名>
```

删除本地分支,当分支还没推送就用-D强行删除

```
git push origin --delete remoteBranchName

// 简写
git push origin :fix/authentication

```
删除远程分支名


**分支命名规范**

- `master `    主分支
- `develop`   开发分支
- `feature/xxx`   新功能分支   
- `bugfix/xxx`   修复 `bug` 分支
- ` hotfix/xxx  ` 热修复分支，修复紧急`bug`
- `release/x.x.x`  新版本分支



### git  checkout 

> 切换分支

```
git checkout <分支名>
```

切换到指定分支上

```
git checkout -b <分支名>
```

新建一个分支并切换到该分支相当于

```
git branch <分支名>
git checkout <分支名>
```


### git tag 

> 为项目标记里程碑

```
git tag <标签名>
```

可在重要节点为项目代码做一个标记



## 总结

上述命令只是`git`的常用命令，把这些掌握了，应对日常开发是完全足够的，遇到一下复杂的情况，`Google`一下即可，记得要把你遇到的问题和解决的方案都记录下来哦，这样再遇到类似问题就能举一反三了，_把每一个踩过的坑都变成你成长的养分_

