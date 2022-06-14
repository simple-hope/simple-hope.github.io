---
layout: post
title:  "《精通git》读书笔记"
date:   2022-06-14 14:18:36 +0800
categories: jekyll update
---


## 3.5 远程分支
远程分支是指针，存在于本地且**无法被移动**。当你与服务器进行网络通信时，它们会自动更新。远程分支有点像书签，它们会提示你上一次连接服务器时远程仓库中每个分支的位置。

git fetch会更新远程分支指针
### 3.5.1 推送
`git push origin serverfix`实际上是`git push origin refs/heads/serverfix:refs/heads/serverfix`

如果不想每次推送时都键入密码，可以设置一个凭据缓存（credential cache）。请参阅[7.14节](#7-14-凭据存储)

上游分支的简单写法：如果你已经设置好了上游分支，就可以通过@{u}来使用。例如在master上用`git merge @{u}`来代替`git merge origin/master`

`git branch -vv`列出每个分支的名字及其跟踪的远程分支信息，以及领先或落后的信息。要注意的是，这条命令和`git status`都不会与服务器通信以获取最新信息。
### 3.5.4 删除远程分支：`git push origin --delete serverfix`

## 3.6 变基
### 3.6.1 变基的工作原理
1. 找到共同祖先
1. 取得每次提交引入的更改（diff）
1. 依次引入更改  

# ch04
如果你对自己运行Git服务器不感兴趣，可以跳过本章的前面几节，直接阅读4.9节。

## 4.1 协议
### 4.1.1 本地协议
使用file://前缀的主要原因是希望取出仓库中的外部引用或多余对象。
### 4.1.2 HTTP协议
智能HTTP协议最普遍。既支持匿名访问，也支持用户验证。
### 4.1.3 SSH协议
SSH不能实现匿名访问
### 4.1.4 Git协议
Git协议不提供任何身份验证方式

## 4.2 在服务器上搭建Git
### 4.2.1 将裸仓库放置在服务器上
如果你只需要与几个人协作完成一个私有项目，那么只需要SSH服务器和裸仓库就够了
### 4.2.2 小型团队配置
配置Git服务器时最麻烦的一个方面就是用户管理

## 4.3 生成个人的ssh公钥
让Git服务器的管理员把公钥添加到git用户的`~/.ssh/authorized_keys`文件中即可访问

## 4.4 设置服务器
每次创建新项目时，都需要有人通过SSH方式登录该服务器并创建一个新的裸仓库

如果该服务器搭建在内网，并且已经配置好DNS，使得gitserver指向该服务器，那么你就可以直接克隆`git@gitserver:/srv/git/project.git`

## 4.5 Git守护进程
无需授权，提供公开项目的访问

## 4.6 智能HTTP
Git自带一个叫做git-http-backend的CGI脚本，你可以调用它来实现通过HTTP协议发送和接收数据

## 4.7 GitWeb
在Linux上lighttpd一般都已经装好，可以直接在项目目录下执行`git instaweb`来通过web展示Git项目和数据

## 4.8 GitLab

每个项目都归属于单个命名空间
1. 用户命名空间
1. 组命名空间

钩子会触发HTTP POST操作，其中携带的描述性信息采用的是JSON格式。以便自动化

# ch05

## 5.1 分布式工作流
### 5.1.1 集中式工作流
中枢接受代码，所有人以此同步各自的工作
### 5.1.2 集成管理者工作流
开发人员fork，集成管理者处理pull request
### 5.1.3 司令官与副官工作流
用于Linux内核等等超大型项目或高度层次化环境。

## 5.2 为项目做贡献
### 5.2.1 提交准则
提交之前执行`git diff --check`列出可能存在的空白字符错误

提交消息的第一行不应该超过50个字符
### 5.2.4 用`rebase -i`将工作内容压缩成单个提交可能会方便维护人员评审补丁。7.6节会对交互式变基做更详尽的介绍。
### 5.2.5 用`git format-patch`来生成要通过电子邮件发送的mbox格式的文件
用`git send-email`之前需要配置
### 5.3.4 确定引入内容
[CF:7.1 选择修订版本](#7-1-选择修订版本)
双点语法：`git log master..contrib`显示在contrib中但不在master中的提交
三点语法：`git diff master...contrib`显示contrib与master分岔后的diff
### 5.3.5 大型合并工作流
将主题分支合并到next并推送共享，如果主题分支仍需改进则合并到pu分支，稳定下来后重新和并入master。这意味着master始终快近，next偶尔变基，pu频繁变基。maint用于提供向后移植补丁。
### 5.3.9 汇总自上次v1.1.1之后所有提交：`git shortlog --no-merges master --not v1.1.1`

# ch06

### 6.3.2 添加协作人员：Collaborators

## 6.5 GitHub脚本化
### 6.5.1 钩子系统
### 6.5.2 GitHub API

# ch07

## 7.1 选择修订版本
### `git log --left-right master...experiment`可以显示出提交属于哪一侧的分支。[CF:5.3.4 确定引入内容](#5-3-4-确定引入内容)

## 7.3 储藏
### 7.3.3 从储藏中创建分支：`git stash branch 新分支名`
### 清理工作目录
`git stash -all`以储藏形式保存全部内容
`git clean -n`做一次删除的演习
`git clean -f`进行实际删除

## 7.5 搜索
### 7.5.1 工作目录搜索
`git grep 模式`默认只查找工作目录下的文件

`git grep --count 模式`输出总结信息：每个匹配的文件中有多少处匹配

`git grep -p 模式`显示所查找到的匹配属于哪个方法或函数

还可以使用--and选项来查找复杂的字符串组合
### 7.5.2 日志搜索
`git log -S 字符串`显示出添加过或删除过该字符串的那些提交

`git log -G 模式`等同于-S但使用正则表达式进行模式匹配

`git log -L 模式:文件`显示所有匹配的改动记录

## 7.6 重写历史
### 7.6.3 重排提交：在交互式变基中改变行的顺序
### 7.6.4 压缩提交：在交互式变基中用squash
### 7.6.5 拆分提交：在交互式变基中用edit
### 7.6.6 超强命令：filter-branch
1. 从所有提交中删除某个文件：`git filter-branch --tree-filter 'rm -f 文件' HEAD`。要想在所有分支上执行filter-branch，可以传入--all选项
2. 将子目录设置为新的根目录：`git filter-branch --subdirectory-filter 目录 HEAD`
3. 全面修改邮件地址：利用shell中的if语句配合GIT_AUTHOR环境变量

## 7.7 重置揭秘：reset和checkout
### 7.7.5 要压缩最新的几个提交，只需`git reset --soft 旧提交`然后commit
### 7.7.7 文件级别与提交级别

|			|索引|工作目录|覆盖是否安全|
|---|---|---|---|
|提交级别||||
|reset --soft COMMIT	||||
|reset --mixed COMMIT	
|reset --hard COMMIT	
|checkout COMMIT	
|文件级别||||
|reset COMMIT FILE	
|checkout COMMIT FILE	

### 7.8.1 合并冲突
`git merge -Xignore-all-space`忽略空白字符的变更

`git checkout --conflict=diff3 冲突文件名`在冲突标记中提供内嵌的base版本

`git checkout`还可以接受--ours和--theirs作为选项，这是一种只选择其中一侧的快速方法。
### 7.8.2 还原提交
对合并提交执行`git revert`需要用-m选项指明要还原哪个父节点引入的变更

如果使用`git revert -m 1 HEAD`来抵消合并，会影响这两个分支以后的合并基础
### 7.8.3 `-Xours`与`-s ours`
给merge传入-Xours或-Xtheirs选项就不再添加冲突标记。任何能够合并的差异，Git会选择合并。任何有冲突的差异，Git会简单地选择你所指定的那一侧。

更严格的策略是`-s ours`。这个方法基本上做的是一次假合并。它会记录一个
#### 读完ch10回来看子树合并

## 7.9 rerere
在有些情况下，重用冲突解决方案相当方便。例如：
+ 想确保一个长期的topic分支能够干净地合并，但又不想要一堆用于**中间阶段**的合并提交。你可以启动rerere功能，偶尔进行合并、解决冲突、**然后退出合并**。如果你一直这么做，那么最终的合并应该会很容易。变基也同理
+ 选用一个已合并的分支，修复了一堆冲突后决定对其进行变基操作，这样你可能就不必再去解决同样的冲突了。
+ 偶尔将多个尚在改进的topic分支合并到一个可测试的头部。如果测试失败，你可以返回合并之前，不使用导致失败的topic分支，然后再次合并，无需再次重新解决冲突。

## 7.10 使用Git调试
### 7.10.1  文件标注
`git blame -L 起始行,结束行 文件`查看每一行的最后一次修改者和修改时间

加入-C参数要求Git找出代码移动的原始出处
### 7.10.2 `git bisect`帮助尽快确定问题是由那、哪一次提交引发的

## 7.11 子模块
### 7.11.1 开始使用子模块
在已有的git仓库中执行`git submodule add https://github.com/chaconinc/DbConnector`会将子项目放入DbConnector文件夹中；还会自动在.gitmodules保存url与路径的映射关系

因为其他用户会首先尝试从.gitmodules文件中的url处进行克隆/获取，所以要确保大家都能访问到该url。如果你用来推送的url和别人拉取时用的url不一样，那么换一个别人能够访问到的。你可以在本地执行`git config submodule.DbConnector.url PRIVATE_URL`来覆盖这个值，以供自己使用。如果可以，采用相对路径是一个不错的选择

执行`git diff --cached DbConnector`会发现git并不会跟踪其中的内容，而是把它看作仓库中的一次特殊的提交。

如果你希望diff的输出美观一些，可以给`git diff`传入`--submodule`选项。如果不想在每次执行`git diff`时都键入`--submodule`，可以设置`git config --global diff.submodule log`

在提交时，会看到`create mode 160000 DbConnector`。模式为160000的rack条目表示你将一次提交作为一个目录项（而非子目录或文件）进行记录
### 7.11.2 克隆含有子模块的项目
克隆时，默认会得到含有子模块的目录，但是目录中并没有文件。你必须分别执行两条命令
1. git submodule init 用于初始化本地配置文件
1. git submodule update 用于从项目中获取所有数据并检出父项目中适合的提交

更简单的做法是在clone时传入`--recursive`选项
### 7.11.3 开发含有子模块的项目
#### 拉取上游变更
进入DbConnector目录执行fetch和merge

返回主项目执行`git diff --submodule`就会看到子模块已经得到了更新

更简单的做法是执行`git submodule update --remote`自动更新所有子模块。该命令默认会假定你要将检出更新为子模块仓库的master分支。可以在两个地方修改
1. `git config -f .gitmodules submodule.DbConnector.branch stable`修改.gitmodules文件（这样其他人也可以跟踪到）
1. 去掉选项`-f .gitmodules`将只修改本地的`.git/config`，只对你有效

**跳过复杂操作**

### 7.11.4 子模块技巧
**读到这里**

## 7.12 打包bundle
## 7.13 替换：历史记录拆分后可用replace重新嫁接
## 7.14 凭据存储
`git config --global credential.helper cache --timeout 900`设置将凭据保存在内存中15分钟

`git config --global credential.helper store --file 路径`设置将凭据保存在硬盘

# ch08 自定义Git

## 8.1 配置Git
### 8.1.1 基本配置
`git config --global commit.template ~/.gitmessage.txt`设置默认的提交消息占位符
### 8.1.3 外部的合并与diff工具
`git config --global merge.tool kdiff3`
### 8.1.4 格式化与空白字符
1. core.autocrlf 处理行终止符。win适合把它置为true，unix适合把它置为input。
2. core.whitespace 用于检测、修正与空白字符相关的问题
+ （默认）blank-at-eol 会查找行尾的空格
+ （默认）blank-at-eof 会查看文件末尾的空行
+ （默认）space-before-tab 会查找行首制表符之前的空格
3. 如果提交了存在空白字符问题的文件，可以执行`git rebase --whitespace=fix`自动修复

## 8.2 Git属性
针对路径的配置被称为Git属性
### 8.2.1 二进制文件
1. `*.pbxproj binary`告诉Git把`*.pbxproj`当做二进制文件来处理
1. 比较二进制文件
+ `*.docx diff=word`配合`git config diff.word.textconv docx2txt`可比较word文件的文字改动
1. `*.png diff=exif`配合`git config diff.exif.textconv exiftool`可比较图像的元信息
### 8.2.2 关键字扩展
1. `*.txt ident`让Git在检出时将blob对象的SHA-1写入$Id$字段。用途有限
2. 编写自己的smudge和clean
















[读完ch10回来看子树合并](#读完ch10回来看子树合并)

