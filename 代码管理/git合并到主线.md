# 合并到主线



## 1、切换到主线main

git checkout main

拉取一下最新的代码
git pull


## 2、把develop分支的代码合并到main上

git merge develop


### 2.1如果有冲突。找到冲突文件打开解决冲突。

<<<<<<< HEAD和=======之间的内容：是master分支修改的内容
=======和>>>>>>> 分支 new_branch之间的内容
解决完冲突，保存。
在当前目录cmd，执行如下命令：
git add 冲突的文件
git commit -m "解决冲突"


## 3、查看执行状态

git status


## 4、执行提交命令

git push origin main


## 5、回到开发分支

git checkout develop