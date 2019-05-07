# # Git 安装

## 1、配置用户信息

配置个人的用户名称和电子邮件地址：

```shell
git config --global user.name  "your Name"
git config --global user.email "youremail@example.com"
```
如果要在某个项目中使用其他名字或者电邮，只要去掉 `--global` 选项重新配置即可，新的设定保存在当前项目的 *.git/config* 文件里。

## 2、查看配置信息

```shell
git config --list
```

# # Git 创建版本库

创建一个文件夹，然后

```shell
git init // 初始化版本库
```

# # Git 基本操作

## 1、添加/删除/提交

```
git add .          // 从工作区修改内容,提交变动到暂存区
git rm test.md	   // 从工作区删除文件,提交变动到暂存区
git commit -m ""   // 从暂存区提交修改到本地分支
```

## 2、查看状态

```shell
git status    // 查看状态
git diff 	  // 查看修改
git log --pretty=oneline  // 查看commit的版本日志(简洁模式)
git log --graph           // 查看版本日志（图形模式）
git reflog    // 查看操作日志
```

## 3、撤销修改

```shell
git checkout -- README.md   // 撤销工作区中指定文件的修改内容
git reset HEAD -- README.md // 撤销暂存区中指定文件
git reset --hard HEAD       // 撤销暂存区中的所有文件
```

## 4、版本回退

```shell
git reset --hard HEAD"^" // 回退到上个版本
git reset --hard 35553a7 // 回退到指定版本号
```

# # 两种方式映射远程仓库

## 1、配置

**第1步：**

创建SSH Key。

在用户主目录下查找.ssh目录，如果有，查看是否存在私钥 ”id_rsa“ 和公钥 ”id_rsa.pub“ 。如果存在，则执行第2步。否则就打开Git Bash，创建SSH Key：

```shell
$ ssh-keygen -t rsa -C "youremail@example.com"
```

**第2步：**

在github中添加SSH Key。

Title：设置任意SSH key 标题

Key：将公钥拖进来

## 2、方式A：将本地项目推送到远程库

1. 初始化本地仓库,将项目推送到本地分支

2. 在github创建远程仓库test

3. 将本地仓库关联远程仓库,并进行第一次推送

   ```shell
   git remote add origin https://github.com/ZhangYachao1024/test.git
   git push -u origin master  // 第一次推送，添加-u参数将本地master分支和远程master分支关联起来
   ```

## 3、方式B：克隆远程库到本地

1. 若没有远程仓库,则需先创建一个远程仓库

2. 用命令`git clone`克隆出一个本地库：

   ```
   git clone git@github.com:ZhangYachao1024/test.git
   ```

# # 分支管理

## 1、分支创建/切换/查看                                           

```shell
git branch dev          // 创建dev分支
git checkout dev        // 切换到dev分支
git checkout -b dev     // 创建并切换到dev分支
git branch 				// 查看当前分支和其他分支
```

## 2、分支合并

```
git merge dev           // 当前分支合并dev分支（快速模式）
git merge dev --no-ff   // 当前分支合并dev分支（普通模式）
```

## 3、 分支删除

```shell
git branch -d dev     
git branch -D dev       // 强行删除
git push origin --delete dev // 删除远程分支
```

## 4、解决冲突

![17](https://user-images.githubusercontent.com/12387544/35031759-4bef2796-fb9f-11e7-877e-3eb54a7cebca.png)

## 5、解决Bug 

临时存储工作区:

```shell
git stash  		// 存储当前工作区
git stash list  // 查看存储的工作区
```

恢复工作区:

1. 用 `git stash apply` 恢复，但是恢复后，stash 内容并不删除，你需要用 `git stash drop` 来删除；
2. 用 `git stash pop`，恢复的同时删除stash内容

## 6、多人协作

```shell
git remote -v    // 查看远程库信息
```

```shell
git checkout -b dev origin/dev  // 创建并切换远程分支到本地
git pull  // 拉取远程分支
git push origin dev  // 推送master到远程分支
```

如果 `git pull` 提示 *“no tracking information”*，则说明本地分支和远程分支的链接关系没有创建，用命令:

```shell
$ git branch --set-upstream branch-name origin/branch-name
```

# # 标签管理

## 1、创建标签

```shell
git tag v1.1            // 为当前commit创建标签
git tag v0.8 8e7e3d8    // 为指定commit创建标签
git show v0.8           // 查看标签信息
git tag -a <tagName> -m <des> <commit id>    // 创建带有说明的标签
```

## 2、操作标签

```shell
git tag -d v1.0         // 删除本地标签
git push origin :refs/tags/v0.5    // 删除远程标签
git push origin v0.8    // 推送某个标签到远程
git push origin --tags  // 推送全部标签到远程
```

# # 同步更新FORK仓库

如果fork源仓库更新了内容，要更新fork之后的内容，操作如下：

1. 查看远程信息

```shell
$ git remote -v
```

2. 添加远程库

```shell
$ git remote add upstream https://github.com/xxx/xxx(fork源仓库地址)
```

3. 从fork源仓库同步更新代码

```shell
$ git fetch upstream
```

4. 合并到本地代码

```shell
$ git merge upstream/master
```

5. 更新并合并自己远程仓库的代码

```shell
$ git pull origin master
```

6. 向自己远程仓库推送刚才同步源仓库后的代码

```shell
$ git push origin master
```

# # GitHub Pages

创建一个仓库，命名为：`git用户名.github.io`

然后就可以往这个仓库里面保存一些项目

假设项目在仓库中的位置：ZhangYachao1024.github.io/project

那么，我们只需要在浏览器中输入如下地址即可直接在线访问项目

-> https://ZhangYachao1024.github.io/project/index.html




















