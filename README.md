### GitUse
	错误处理：
	1.Permission denied (publickey).
		1.ssh-agent
		2.ssh-add~/.ssh/id_key
	2.[git] warning: LF will be replaced by CRLF | fatal: CRLF would be replaced by LF
		遇到这两个错误，是因为Git的换行符检查功能。
		core.safecrlf

	Git提供了一个换行符检查功能（core.safecrlf），可以在提交时检查文件是否混用了不同风格的换行符。这个功能的选项如下：
	false - 不做任何检查
	warn - 在提交时检查并警告
	true - 在提交时检查，如果发现混用则拒绝提交
	建议使用最严格的 true 选项。
	core.autocrlf
	假如你正在Windows上写程序，又或者你正在和其他人合作，他们在Windows上编程，而你却在其他系统上，在这些情况下，你可能会遇到行尾结束符问题。这是	因为Windows使用回车和换行两个字符来结束一行，而Mac和Linux只使用换行一个字符。虽然这是小问题，但它会极大地扰乱跨平台协作。
	Git可以在你提交时自动地把行结束符CRLF转换成LF，而在签出代码时把LF转换成CRLF。用core.autocrlf来打开此项功能，如果是在Windows系统上，把它设	置成true，这样当签出代码时，LF会被转换成CRLF：
	
	```$ git config --global core.autocrlf true```
	
	Linux或Mac系统使用LF作为行结束符，因此你不想 Git 在签出文件时进行自动的转换；当一个以CRLF为行结束符的文件不小心被引入时你肯定想进行修正，把	core.autocrlf设置成input来告诉 Git 在提交时把CRLF转换成LF，签出时不转换：
	```$ git config --global core.autocrlf input```
	这样会在Windows系统上的签出文件中保留CRLF，会在Mac和Linux系统上，包括仓库中保留LF。
	如果你是Windows程序员，且正在开发仅运行在Windows上的项目，可以设置false取消此功能，把回车符记录在库中：
	```$ git config --global core.autocrlf false```

创建用户
```
	git config -global user.name"LzCrazy"
	git config --global user.eamil "mizur@sian.com"
```
 
删除隐藏目录
	```
	rm -rf
	``` 

将文件拉到本地文件夹中
```
git pull --rebase origin master

```

#### 1.创建版本库
	pwd:显示当前目录
	git init:将目录变成管理仓库(.git,使用 ls -ah查看)
	1.将文件添加到版本库（repository）
		1.git add 文件：添加文件
		2.git commit -m "第一次添加到文件"：将文件提交到仓库
		3.git status:查看当前仓库的状态
		4.git diff:查看上一次提交修改的情况
		5.git log：查看所有提交的日志
		6.git reset --hard HEAD^:回退到上一个版本
			 HEAD指向当前版本
		7.git reflog:查看历史，确定回到那个版本
		
		
	2.工作区和暂存区
		1.工作区
			指向的是当前目录的文件夹
			GIT自动创建一个分支master，以及指向master的一个指针叫HEAD
		2.暂存区
			提交到.git目录中的文件
			stage暂存区
			1.git add：添加的文件是在暂存区
			2.git commit：就是往master分支上提交更改
	3.管理修改
		1.cat read.md:修改改文件
		注意：每次修改，如果不add到暂存区，就不会加入到commit中
	4.撤销修改
		1.git checkout --file：丢弃工作区的修改
		 注意：直接丢弃工作区的修改：git checkout --file
		 	  文件已经添加到暂存区，丢弃修改，1.git reset HEAD file 2.git checkout --file
	5.删除文件
		git rm:直接在文件管理器删除文件
		git commit:再次提交

#### 2.远程仓库
	1.远程仓库
		1.创建SSH Key(id_rsa,id_rsa.pub)
			ssh-keygen -t rsa -C "mizur@sina.com"
		2.将id_rsa.pub内容复制到GitHub的ssh中
	2.添加远程仓库
		1.将内容推送到GitHub中
			git remote add origin git@github.com:LzCrazy/javascript.git
		2.将本地内容推送GitHub
			git push -u origin master
				push:把当前分枝master推送到远程
				-u:由于是第一次推送
	3.查看与远端地址或配置
		git remote -v  git config --list
	4.远程连接状态
		ssh -T git@github.comd 

#### 3.分枝管理
	自己的分支的干活，干完一次行整合
	1.创建与合并分支
		master是主分支，HEAD是当前分枝
		1.创建dev分支，然后切换到dev分支
			git checkout -b dev
				-b：表示创建并切换，相当于：
					git branch dev
					git checkout dev
		2.查看当前分支
			git branch
		3.切换到分支
			git checkout master
		4.合并分支到master分支上
			git merge dev
				merge合并指定分支到当前分枝
		5.删除分支
			git branck -d(D) dev
	2.解决冲突
		Your branch is ahead of 'origin/master' by 1 commit.
		在master分支上吧read.txt文件最后一行修改"and"改为"&"
		1.合并分支
			Auto-merging readme.txt
			CONFLICT (content): Merge conflict in readme.txt
			Automatic merge failed; fix conflicts and then commit the result
			出现冲突
				Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容
				将“&“修改成“and"
		2.查看分支合并图
			git log --graph
	3.分支管理策略
		git会用fast forward模式，但这种模式下，删除分支后，会丢掉分支信息
		如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样分支历史就可以看出分支信息
		1.创建分支并切换
			git checkout -b dev
			提交一个新的commit
			git add read.txt
			切换至master
			git checkout master
			分支合并，禁用Fast forward
			git merge --no-ff -m "merge wieh no-ff" dev
		2.Bug分支
			文件出现bug，需要修复，但文件还没有提交的情况
			将工作的文件收藏起来：
			git stash
			查看没有提交的文件bug
			git stash list
			恢复bug内容
			git stash apply;其中stash内容没有删除，需要git stash drop来删除
			或者：git stash pop,恢复的同时把stash内容也删除了
			查看指定的stash内容
			git stash apply stash @{0}
		3.Feature分支
			添加一个新的功能，最好新建一个feature分支，完成后，合并，最后删除feature分支
			添加一个feature分支
			git checkout -b feature1
			完成功能开发
			git add index.js
			git status
			切回dev
			git checkout dev
			新功能取消
			git branch -d feature1
			销毁失败，是因为feature1分支没有合并，可以采用强行删除
			git branch -D feature1
		4.多人协助
			查看远程库
			git remote
			查看详细信息
			git remote -v
			推送分支
			git push origin master
			推送其他分支
			git push origin dev
			推送需要的内容
				master分支是主分支，时刻与远程同步
				dev开发分支，需要同步
				bug修复本地，可推送，可不推送
				feature，随便自己推送
			抓取推送
			 git checkout -b dev origin/dev
			将最新提交的内容抓取
				git pull  本地合并，解决冲突，再次推送
				pull失败（没有指定本地dev分支和远程origin/dev分支的链接），再pull
#### 4.标签管理
	1.创建标签
		切换到需要打标签的分支上
		git branch 
		git checkout master
		新标签
		git tag v1.0
		查看标签
		git tag
	默认的标签是打在最新提交的commit上
	补打历史标签
	git log --pretty=online --abbrev-commit
	git tag v0.9 2267787
	标签顺序上字母排序，查看标签信息
	git show <tagname>
	创建带有说明的标签
	git tab -a v1.0 -m "version1.0" 321312
		-a指定标签名，-m指定说明文字
	给私钥一个标签
	git tag -s v1.0 -m "signed version1.0" eaffa132

	2.操作标签
		删除标签
			git tag -d v1.0
		推送标签到远程
			git push origin <tagname>
		全部标签推送到远程
			git push origin --tag
		删除远程推送
			先删除本地推送
			git tag -d v1.0
			删除远程推送
			git push origin :refs/tags/v1.0

#### 5.使用github
#### 6.自定义git
	修改显示颜色
	git config --global color.ui true
	忽略特殊文件
		git add App.class
	添加文件到GIT
		git add -f App.class
			-f强制添加到git
	配置别名
		git status==>git config --global alias.st status
		git st

#### 7.搭建git服务器
	1.安装git
		sudo apt-get install igt 
	2.创建git用户,运行git服务
		sudo adduser git
	3.创建证书登陆
		收集所有需要登陆的用户的公钥，就是"id_rsa.pub"把所有的公钥导入到/home/git/.ssh/authorzed_keys文件中
	4.初始化git
		sudo git init --bare sample.git
		初始化的git没有工作区，主要是为了共享，修改自己的git文件.git结尾改成git
			sudo chown -R git:git sample.git
	5.禁用shell登陆
		编辑文件/etc/passwd
		查找：git:x:1001:1001:...:/home/git:/bin/bash
		修改为：git:x:1001:1001:...:/home/git:/usr/bin/git-shell
	6.克隆远程仓库
		git clone git@server:/srv/sample.git
		























