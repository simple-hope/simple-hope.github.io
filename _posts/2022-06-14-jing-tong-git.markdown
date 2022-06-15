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
如果你对自己运行git服务器不感兴趣，可以跳过本章的前面几节，直接阅读4.9节。

## 4.1 协议
### 4.1.1 本地协议
使用file://前缀的主要原因是希望取出仓库中的外部引用或多余对象。
### 4.1.2 HTTP协议
智能HTTP协议最普遍。既支持匿名访问，也支持用户验证。
### 4.1.3 SSH协议
SSH不能实现匿名访问
### 4.1.4 git协议
git协议不提供任何身份验证方式

## 4.2 在服务器上搭建git
### 4.2.1 将裸仓库放置在服务器上
如果你只需要与几个人协作完成一个私有项目，那么只需要SSH服务器和裸仓库就够了
### 4.2.2 小型团队配置
配置git服务器时最麻烦的一个方面就是用户管理

## 4.3 生成个人的ssh公钥
让git服务器的管理员把公钥添加到git用户的`~/.ssh/authorized_keys`文件中即可访问

## 4.4 设置服务器
每次创建新项目时，都需要有人通过SSH方式登录该服务器并创建一个新的裸仓库

如果该服务器搭建在内网，并且已经配置好DNS，使得gitserver指向该服务器，那么你就可以直接克隆`git@gitserver:/srv/git/project.git`

## 4.5 git守护进程
无需授权，提供公开项目的访问

## 4.6 智能HTTP
git自带一个叫做git-http-backend的CGI脚本，你可以调用它来实现通过HTTP协议发送和接收数据

## 4.7 GitWeb
在Linux上lighttpd一般都已经装好，可以直接在项目目录下执行`git instaweb`来通过web展示git项目和数据

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
给merge传入-Xours或-Xtheirs选项就不再添加冲突标记。任何能够合并的差异，git会选择合并。任何有冲突的差异，git会简单地选择你所指定的那一侧。

更严格的策略是`-s ours`。这个方法基本上做的是一次假合并。它会记录一个
### 子树合并
git能够聪明地发现一个子目录是另一个的子树

这给我们提供了一种方法，可以在不使用子模块的情况下拥有一种类似于子模块流程的工作方式。其不足之处在于略有些复杂，容易出现操作错误

## 7.9 rerere
在有些情况下，重用冲突解决方案相当方便。例如：
+ 想确保一个长期的topic分支能够干净地合并，但又不想要一堆用于**中间阶段**的合并提交。你可以启动rerere功能，偶尔进行合并、解决冲突、**然后退出合并**。如果你一直这么做，那么最终的合并应该会很容易。变基也同理
+ 选用一个已合并的分支，修复了一堆冲突后决定对其进行变基操作，这样你可能就不必再去解决同样的冲突了。
+ 偶尔将多个尚在改进的topic分支合并到一个可测试的头部。如果测试失败，你可以返回合并之前，不使用导致失败的topic分支，然后再次合并，无需再次重新解决冲突。

## 7.10 使用git调试
### 7.10.1  文件标注
`git blame -L 起始行,结束行 文件`查看每一行的最后一次修改者和修改时间

加入-C参数要求git找出代码移动的原始出处
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

### 7.11.4 foreach与别名
`git submodule foreach 'git diff'`

### 7.11.5 子模块的问题
在分支A中加入子模块后切换回没有子模块的分支B，会留下一个未被跟踪的子模块目录。

如果删除该目录，然后切换回分支A，需要执行`submodule update --init`来重新填充

另一个要重点注意的地方涉及从子目录到子模块的切换

## 7.12 打包
`git bundle create repo.bundle HEAD master`创建的repo.bundle文件包含了重建仓库master分支所需的所有数据

`git clone repo.bundle repo`可以从二进制文件克隆到一个目录中。如果在create时没有在引用中包含HEAD，那还得指定`-b master`或者其他被引入的分支

也可以只打包部分提交，与format-patch的区别在于只生成一个文件

## 7.13 替换：历史记录拆分后可用replace重新嫁接

## 7.14 凭据存储
git凭据系统提供的选项
1. 默认不缓存任何内容。所有连接都会提醒你输入输入用户名和密码
1. cache模式会将凭据保存在内存15分钟，绝不会将密码存储在磁盘上。
1. store模式将凭据保存在磁盘上的纯文本文件中，且永不过期。
1. Mac还有osxkeychain模式，会把凭据以加密形式存放在磁盘上
1. Win可以安装`Git Credential Manager for Windows`使用`Windows Credential Store`来控制敏感信息

`git config --global credential.helper cache --timeout 30000`设置将凭据保存在内存中约8小时

`git config --global credential.helper store --file 路径`设置将凭据保存在硬盘

如果你的凭据文件保存在u盘上，希望在没有插入u盘的时候使用内存缓存来保存凭据。那么.gitconfig看起来如下所示
```
[credential]
	helper = store --file /mnt/thumbdrive/.git-credentials
	helper = cache --timeout 30000
```
### 7.14.1 底层实现
### 7.14.2 自定义凭据缓存

# ch08 自定义git

## 8.1 配置git
### 8.1.1 基本配置
`git config --global commit.template ~/.gitmessage.txt`设置默认的提交消息占位符

如果你要创建签署过的附注标签（在7.4节中讨论过），那么将你的GPG签署密钥作为配置项设置会更方便。设置
`git config --global user.signingkey <gpg-key-id>`
以后再签署标签时只需`git tag -s <tag-name>

core.excludesfile可用于设置系统级ignore

将help.autocorrect设置为1可以在输错命令时将git推测出的命令自动执行；将其设置为50会在执行纠正命令前给你留出5秒钟的考虑时间
### 8.1.2 自定义配色
### 8.1.3 外部的合并与diff工具
`git config --global merge.tool kdiff3`
### 8.1.4 格式化与空白字符
1. core.autocrlf 处理行终止符。win适合把它置为true，unix适合把它置为input。
2. core.whitespace 用于检测、修正与空白字符相关的问题
+ （默认）blank-at-eol 会查找行尾的空格
+ （默认）blank-at-eof 会查看文件末尾的空行
+ （默认）space-before-tab 会查找行首制表符之前的空格
3. 如果提交了存在空白字符问题的文件，可以执行`git rebase --whitespace=fix`自动修复

### 8.1.5 服务器配置
设置`git config --system receive.fsckObjects true`能够确认在推送过程中接收到的每一个对象的有效性以及是否匹配其SHA-1校验和。但代价太高，有可能拖慢其他操作，尤其是在处理大型仓库或推送大文件的时候。

设置`git config --system receive.denyNonFastForwards true`可以拒绝`push -f`。另一种实现方法是通过服务器端的接收钩子，能够让你完成一些更复杂的操作

有一种方法可以绕开denyNonFastForwards：先删除分支然后再推送。为了避免这种情况，可以将receive.denyDeletes设置为true禁止删除远程分支

## 8.2 git属性
针对路径的配置被称为git属性。如果你不希望将属性文件连同项目一起提交，也可以在.git/info/atttibutes文件中设置
### 8.2.1 二进制文件
1. `*.pbxproj binary`告诉git把后缀为.pbxproj的文件当做二进制文件来处理
1. 比较二进制文件
+ `*.docx diff=word`配合`git config diff.word.textconv docx2txt`可比较word文件的文字改动
+ `*.png diff=exif`配合`git config diff.exif.textconv exiftool`可比较图像的元信息
### 8.2.2 关键字扩展
`*.txt ident`让git在检出时将blob对象的SHA-1写入$Id$字段。用途有限

可以编写自己的smudge和clean后，在文件检出时会运行smudge过滤器，在文件暂存时会运行clean过滤器
### 8.2.3 有2个属性用于git archive
### 8.2.4 合并时忽略特性分支的变更
设置`git config --global merge.ours.driver true`并添加属性`db.xml merge=ours`可以让db.xml文件不受特性分支影响

## 8.3 钩子
### 8.3.1 安装钩子
想启用一个钩子脚本，需要将不含.sample后缀的可执行文件放入.git/hooks
### 8.3.2 客户端钩子
#### 提交工作流钩子
1. pre-commit钩子在键入提交消息前运行。如果以非零值退出，提交会被中止，但可以使用`git commit --no-verify`来绕过这个环节
1. prapare-commit-msg钩子的运行时机是在提交消息编辑器启动之前，默认消息被创建之后。一般用于 那些自动生成默认消息的提交。如
+ 提交消息模板
+ 合并提交
+ 压缩提交
+ 修正提交
1. commit-msg钩子用于在提交通过前验证项目状态或提交信息。在8.4节演示
1. post-commit钩子在提交结束后运行，通常用于通知

#### 基于电子邮件的3个钩子都是由git am调用的
#### 其他客户端钩子
1. pre-rebase钩子在变基前运行，如果以非零值退出，那么变基过程就会被挂起
1. post-rewrite钩子由那些执行提交替换的命令调用，包括--amend和rebase，但不包括filter-branch。该脚本的唯一参数是触发重写的命令名称，它从stdin中接收一系列重写的提交。
1. post-checkout钩子在checkout成功执行后被调用，可用于移入不想纳入版本控制的大型文件
1. post-merge钩子会在merge成功执行后运行，可用于
+ 恢复git无法跟踪的权限数据
+ 验证是否有不受git控制的文件存在
1. pre-push钩子的调用时机是在远程引用被更新之后，尚未传输对象之前。如果以非零值退出，中止推送
1. pre-auto-gc钩子会在git gc开始前被调用

### 8.3.3 服务器端钩子
在推送之前运行的服务器端钩子可以随时以非零值退出，拒绝推送操作，并在客户端输出错误提示
1. pre-receive在收到推送时首先运行，它会从stdin处接收一系列被推送的引用
1. update脚本类似于pre-receive，但它会为每一个要更新的分支都运行一次。它不会从stdin处读取内容，它接受3个参数：引用名称（分支）、推送之前的SHA-1、推送内容的SHA-1。如果以非零值退出，只有对应的引用会被拒绝，其他引用仍然会被更新
1. post-receive钩子无法阻止推送过程，但在推送结束之前会一直保持连接，因此进行一些耗时较长的操作需谨慎

## 8.4 git强制策略示例
用Ruby编写脚本实现：检查提交消息的格式，只允许特定的用户修改项目中特定的子目录

# ch09 git与其他系统

### 9.1.1 git svn与Subversion双向桥接
建议保持线性：只变基不合并

不要向单独的git服务器推送任何不包含git-svn-id的内容

**跳过**

### 9.2.5 自定义导入工具
git fast-import从stdin中读取简单的指令来写入特定的git数据。比起执行原始的git命令或是编写原始对象要容易

# ch10 git内幕

git本质上是一个可按内容寻址的文件系统

## 10.1 底层命令和高层命令
.git目录中，有4项很重要
1. objects存储了个人数据库的所有内容
1. refs存储指针，指向提交对象
1. HEAD指向当前检出的分支
1. index保存暂存区信息

## 10.2 git对象
git的核心就是“键-值”数据存储。

blob对象保存文件内容
### 10.2.1 树对象
树类型对象解决了存储文件名的问题
### 10.2.2 提交对象
提交对象指定单个树对象的SHA-1、父提交对象和提交信息
### 10.2.3 对象存储
git构造出以对象类型作为开头的头部信息，例如"blob #{content.length}\0"，将头部信息和原始内容拼接在一起，然后计算新内容的SHA-1；接着使用zlib压缩新内容；最后写道磁盘上的对象中

## 10.3 git引用
1. `.git/HEAD`包含指向当前所在分支的符号引用；
1. `.git/refs/heads/`存储分支；
1. `.git/refs/tags/`存储标签；
1. `.git/refs/remotes/`存储最后一次推送到远程仓库的每个分支的值。是只读的，无法用commit命令来更新

## 10.4 包文件
执行git gc会查找名称和大小相近的文件，只保存不同版本文件之间的差异。

包文件包含了删除的松散对象，索引文件包含了针对包文件内容的偏移（**博主注：这似乎说明在加密文件系统中使用大型包文件可能会让索引记录的偏移失去意义，因为需要解密整个包文件**CF：[维护](#10-7-1-维护)）
+ `.git/objects/info/packs`
+ `.git/objects/pack/pack-*.idx`
+ `.git/objects/pack/pack-*.pack`

底层命令`git verify-pack`可以用来查看打包的内容

## 10.5 引用规格
`git remote add origin <url>`会在.git/config中添加3行
```
[remote "origin"]
        url = <url>
        fetch = +refs/heads/*:refs/remotes/origin/*
```
其中的加号指示即便在不能快进的情况下也更新引用

可以通过添加一行`push = refs/heads/master:refs/remotes/qa/master`使得这成为`git push origin`的默认操作

## 10.6 git传输协议
### 10.6.1 哑协议
dumb协议的获取过程就是一系列的HTTP GET请求

替代仓库是让派生项目共享对象的好办法。`objects/info/http-alternates`记录替代仓库URL列表
### 10.6.2 智能协议
用ssh协议push时会尝试以ssh的方式在远程服务器上执行命令，就像
`ssh -x git@server "git-receive-pack 'simplegit-progit.git'"`

收到服务器的状态后，send-pack进程就能够确定那些提交是自己拥有但服务器却没有的

## 10.7 维护与数据恢复
### 10.7.1 维护
有7000个左右的松散对象或50个以上的包文件会触发gc，可以分别修改gc.auto和gc.autopacklimit配置设置来修改这些限制
### 10.7.2 数据恢复
`git reflog`展示HEAD曾经指向的引用

`git log -g`会将引用日志按照正常的log格式输出

如果丢失了引用日志，例如误操作`rm -Rf .git/logs/`，可以使用`git fsck`检查数据的完整性，加上--full选项会显示出所有没被其他对象指向的对象
### 10.7.3 移除大对象
#### 首先找到哪些文件较大
先执行git gc把所有对象放到包文件中（博主注：包括二进制文件）

然后用`git count-objects -v`查看是否只有1个包文件

接着用`git verify-pack -v .git/objects/pack/pack-*.idx | sort -k 3 -n | tail`列出最大体积的对象。要查出文件名，还要用`git rev-list --objects --all | grep <sha1>`
#### 查看哪些提交修改了该大文件
`git log --oneline --branches -- <path>`
#### 移除
```
git filter-branch -index-filter \
	'git rm --ignore-unmatch --cached <path>' -- <引入大文件的最早提交>^..
```

其中--index-filter选项与7.6节用过的--tree-filter类似，但它配合`git rm --cached`速度更快。--ignore-unmatch选项指明待删除文件不匹配时不显示错误

### 10.8 环境变量


[读完ch10回7.8.3看子树合并](#读完ch10回来看子树合并)

