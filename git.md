# <center>git总结

>参考链接：<https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000>
<h3>创建版本库</h3>
	
选择一个合适的地方，创建一个空目录：
	
	1、$ git init //初始化一个仓库 
	2、创建一个文件readme.txt（千万不要使用Windows自带的记事本编辑任何文本文件，建议你下载Notepad++代替记事本。）
	2、$ git add readme.txt
	3、$ git commit -m "wrote a readme file" //-m 添加一个有意义的提交描述
	4、$ git remote add origin https://github.com/dongruinan/FE-note.git //关联远程仓库
	5、$ git push -u origin master //提交到远程仓库

<h3>git 配置用户</h3>

	git config --global user.name [username]
	git config --global user.email [email]

<h3>时光穿梭机</h3>
	1、我们已经成功地添加并提交了一个readme.txt文件，现在，是时候继续工作了，于是，我们继续修改readme.txt文件。

	2、git status命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
	
	3、git diff顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，
<h4>版本回退</h4>
	1、git log命令显示从最近到最远的提交日志，我们可以看到3次提交记录。
    2、如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数
		分别显示版本号  和提交的描述
		fae107990bb3d983f768745587d219342b1dc7a5 xiugai
		628572fa4e01f79d3e47b92229c2f83137b726ac test
		36f5b2e81c219fe6f9d0c7ffa967281cda424cae 观察者模式
	3、现在我们启动时光穿梭机，准备把readme.txt回退到上一个版本，怎么做呢？

	首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本,上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
	4、回到上一个版本（$ git reset --hard HEAD^）
	5、cat readme.txt 查看文件内容 确定回退是否成功
	6、$ git reset --hard 628572f 指定回到哪个版本（版本号不用写全，git会自动查找，尽量多写几位）
	7、如果窗口没有关闭，找到回退前的版本号，直接切换过去。
	8、Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是修改了HEAD指向
	9、你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？
		$ git reflog 用来记录你的每一次命令
		$ git reflog
			fae1079 HEAD@{0}: reset: moving to fae107
			628572f HEAD@{1}: reset: moving to HEAD^
			fae1079 HEAD@{2}: commit: xiugai
			628572f HEAD@{3}: commit: test
			36f5b2e HEAD@{4}: commit (initial): 观察者模式
		可以通过 版本ID 再次乘坐时光机~
	
<h4>工作区和缓存区</h4>

	前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
	
	第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
	
	第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
	
	因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
	
	你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

<h4>撤销修改------------未实验成功</h4>
	
	$ git checkout -- file可以丢弃工作区的修改

	$ git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

<h4>删除文件</h4>
	$ git rm file
	$ git commit -m "remove readme.txt"
	$ git push origin master 

<h3>分支管理</h3>
	
1、首先，我们创建dev分支，然后切换到dev分支：
	
	$ git checkout -b dev
	git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
	$ git branch dev
	$ git checkout dev


2、 git branch命令会列出所有分支，当前分支前面会标一个*号。

3、分支工作完成后，需要先add 文件  commit到 添加到缓存区，再提交到分支，然后切换到主干，使用git merge合并分支内容，合并后在add,commit,push master


	Git鼓励大量使用分支：
	
	查看分支：git branch
	
	创建分支：git branch <name>
	
	切换分支：git checkout <name>
	
	创建+切换分支：git checkout -b <name>
	
	合并某分支到当前分支：git merge <name>
	
	删除分支：git branch -d <name>

<h4>冲突解决</h4>
	1、在分支修改提交后，切换到主干，在主干修改相同文件，造成冲突
		a. 使用 $ git checkout filename;放弃工作区修改，直接合并分支
	

<h4>bug分支</h4>
	当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：
	Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
	$ git stash
	保存工作区 后  进行切换bug分支，修改 提交  合并  删除分支
	切换到工作分支，git status工作区是干净的，刚才的工作现场存到哪去了？用 $ git stash list命令看看：
	工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

	一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

	另一种方式是用git stash pop，恢复的同时把stash内容也删了
	你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

	$ git stash apply stash@{0}
	

<h4>标签管理</h4>
	it的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。
	
	命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

	git tag -a <tagname> -m "blablabla..."可以指定标签信息；

	命令git tag可以查看所有标签。
	
	默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，
	创建标签
	比方说要对add merge这次提交打标签，它对应的commit id是6224937，敲入命令：
	$ git tag v0.9 6224937

<h3>git其他命令总结</h3>
+ git add -A   增加所有改变（新增）文件






