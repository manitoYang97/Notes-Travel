## 批量删除分支

1. 删除**一条分支**：

> git branch -D branchName

2. 删除当前分支外的**所有分支**：

> git branch | xargs git branch -d

3. 删除分支名包含**指定字符的分支**：

> git branch | grep 'dev*' | xargs git branch -d
> 该例将会删除分支名包含’dev’字符的分支。

### 命令解释

`|`
管道命令，用于将一串命令串联起来。前面命令的输出可以作为后面命令的输入。

`git branch`
用于列出本地所有分支。

`grep`
搜索过滤命令。使用正则表达式搜索文本，并把匹配的行打印出来。

`xargs`
参数传递命令。用于将标准输入作为命令的参数传给下一个命令。

管道命令与xargs命令的区别：

管道是实现“将前面的标准输出作为后面的标准输入
xargs是实现“将标准输入作为命令的参数


## 查看修改用户名邮箱

1. 查看当前用户名和邮箱
   
   > git config user.name
   > git config user.email

2. 修改当前用户名和邮箱

   > git config --global user.name <your name>
   > git config --global user.email  <your email>


## 仓库添加公钥

1. 本地已有公钥

```bash
   # ~ 路径下查看.ssh目录
   ls .ssh/
   
   #  id_rsa 私钥      id_rsa.pub 公钥
   
   # 查看公钥内容
   cat .ssh/id_rsa.pub

   # 把公钥内容添加到仓库 setting>SSH Key
```

2. 本地没有公钥，生成公钥

```bash

 ssh-keygen -t RSA -C "user@126.com"

# -t RSA 参数表示使用 RSA 算法
# -C 参数指定用户的电子邮箱地址
# 之后操作同1
```