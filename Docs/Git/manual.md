# ![git logo](./svg/git.svg ':size=60') Git操作手册

## 1.初始化设置
在git命令行内输入以下代码：引号内的填写你注册github时的用户名和电子邮件。
```shell
git config --global user.name "your name"
git config --global user.email "your_email@163.com"
```

## 2.添加SSH Key
首先在本地创建ssh key。在刚刚新建好的文件夹内点击右键Git Bash Here进入git命令行。
```shell
ls -al ~/.ssh 
# 检查是否已经有key了

cd ~/.ssh
# 跳转至C盘用户下的.ssh目录下

ssh-keygen -t rsa -C "your_email@163.com"
# 生成key
```
>“your_email@163.com”改成自己注册github时的邮箱，此处不一定要用163邮箱。  
>回车之后会要求确认路径和输入密码，直接一路回车就行。  
>成功的话会在~/下生成.ssh文件夹，进去打开id_rsa.pub，复制里面的key。

## 3.将Key添加到Github
进入Github，点击`右上角头像`——`Setting`——`SSH and GPG keys`——`New SSH key`，标题可以不写，把`.pub`文件内的代码，复制进去并保存。

## 4.建立本地仓库
在需要上传的文件根目录下运行`Git bash`，输入以下命令
```shell
git init
# 创建git缓存文件夹内，该文件夹是隐藏文件
```

## 5.连接远程Github仓库
在Github新建Repository，创建时不要添加`README`，也不要添加`gitignore` 
同时会生成`SSH`和`HTTPS`两种链接
```shell
git remote add origin git@github.com:fang-king/Selenium.git
# 用git@的格式上传需要参照步骤2设置好SSH

git remote add origin https://ithub.com:fang-king/Selenium.git
# 该方式无需设置SSH，但可能会需要输入账号密码
```

## 6.第一次上传
上传文件之前需先将文件上传至步骤1建立的`git缓存区`，
```shell
git status
# 查看当前未上传的所有修改过文件

git add .
# 上传所有文件至缓存区

git add xxxx.md
# 上传某个文件至缓存区

git commit -m "备注提交信息"
# 提交缓存区文件至仓库区
git commit [file1] [file2] ... -m [message]
# 提交暂存区的指定文件到仓库区

git commit xxxx -a '备注提交信息'                                      
# 提交工作区自上次commit之后的变化，直接到仓库区

git commit --amend -m '备注提交信息'                               
# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit --amend [file1] [file2] ...
# 重做上一次commit，并包括指定文件的新变化

git branch -M main
# 创建main的分支，这一步其实可以跳过，因为现在Git默认分支即为main

git push -u origin main
# 将文件上传至远程仓库的main分支
```

## 7.后续上传
如果要上传的文件实是在一个新的文件夹，那么就需要在该文件所在的文件夹内重新执行`git init`和`git remote`的命令，然后再执行`第六步相关内容`

## 8.拉取远程仓库
第一种：拉取远程，并与本地进行比较，比较后再进行合并
```shell
git remote -v
# 查看当前远程的版本

git fetch origin main
# 获取最新代码远程仓库的main分支

git log -p main..origin/main
# 查看本地的main和远程拉来的main的区别

git merge origin/master
# 合并远程main和本地的main
```

第二种：拉取远程直接与本地合并
```shell
git pull origin main
# 获取远程分支main并merge到当前分支
```

## 9.删除分支
第一种：删除远程分支
```shell
git branch -r
# 查看远程分支

git push origin --delet main
# main是为远程分支名称
```

第二种：删除本地分支
```shell
git branch
# 查看本地分支

git branch -d main
# main为本地分支名
```

## 10.更多Git命令
更多命令，请见一下地址  
https://gist.github.com/guweigang/9848271  
https://gitee.com/all-about-git  
https://blog.csdn.net/XH_jing/article/details/121900458