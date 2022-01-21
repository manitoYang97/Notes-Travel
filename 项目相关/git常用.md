##### git常用

1. #### 一般流程

   - git status  查看项目状态
   - git checkout -b login   新建login分支
   - git branch  查看分支
   - git add .   添加所有文件
   - git commit -m "说明内容"
   - git checkout master 合并之前一定要先切换到主分支
   - git merge login 在主分支合并login分支
   - git push 将本地的master推送到远端
   - git checkout login  切换到子分支
   - git push -u origin  login 把login分支推送到远程

   

2. #### 自己在一个分支编写代码，当要上传代码，开发分支有更新时，需要合并

   - git add .
   - git branch --set-upstream-to=origin/feature/restore feature/restore  关联
   - git checkout develop
   - git pull
   - git checkout feature/restore
   - git merge develop
   - 解决冲突>>> <<<
   - git commit --no-verify
   - git push origin feature/restore

3. #### 想要暂存区是干净的

   - git add .
   - git stash  保持更改
   - git stash pop 取出更改

4. #### 当已经commit之后发现，还没pull远端最新代码

   - git reset HEAD~1 撤销最近一次提交记录

5. #### 关于分支

   - git push origin :home 删除远程分支
   - git branch -D home 删除本地分支
   - git checkout -b feature/new-home  新建分支
   - git branch -a  查看本地分支
   - git push origin feature/new-home:feature/new-home 分支推送到远程

6. #### 提交时不检查代码

   - git commit -m "xxx" --no-verify  不检查提交

7. #### commit规范

   - fix：修复了一个 bug（对应语义化版本中的 Patch）
   - feat：新增了一个功能（对应语义化版本中的 MInor）
   - refactor：重构某块代码
   - perf：改进性能
   - docs：文档相关
   - test：测试相关
   - ci：CI/CD 相关
   - chore：其他类型

8. #### 分支名规范

   - master
   - develop
   - feature/xxx
   - bugfix/xxx
   - hotfix/xxx
   - release/x.x.x

9. #### 更新镜像

   - docker login
   - 打开docker
   - make release
   - 在grds-web.pem文件下操作
     - chmod 400 grds-web.pem
     - SSH -i grds-web.pem centos@54.215.73.75
     - su - root

     - A1isrmQA123
     - kg get pod
     - 找到 squids-cloud-site-xxxx 的名称，复制
       - kg delete pod squids-cloud-site-xxxx