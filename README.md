# Projects
Study and Communication

Git 的分支类型

1.Master 分支
一个项目的代码库应该有且只有一个主分支，所有提供给用户使用的正式版本都在这个主分支上。

2.Develop 分支
此分支为日常使用的开发分支。
这个分支可以用来生成代码的隔夜版本，如果开发功能测试完成后，想正式对外发布，就在 Master 分支上，对 Develop 分支进行 Merge(合并)。

3.临时分支
除了日常开发设置分支外，还有一种临时分支，以应对一些特定目的的版本开发。
(a)功能分支
其是为了开发某种特定的功能，从 Develop 分支上分出来的。开发完成后，合并到 Develop 分支上。其命名规范：feature-*；

(b)预发布分支
在发布到正式版本前，我们可能需要一个预发布版本进行测试。预发布分支是从 Develop 分支上分出来的，预发布结束后，必须合并到 Develop 分支和 Master 分支。其命名规范：release-*；

(c)修补 Bug 分支
软件正式发布后，出现一些 Bug，这时就需要创建一个分支，来进行 Bug 修复。修复 Bug 分支是从 Master 分支上分出来的，修补结束后，在合并到 Develop 分支和 Master 分支。其命名规范：fixbug-*。


Git 的分支使用

1.develop 分支(develop)
(a)Git 创建 develop 分支
develop 分支是从 master 分支中分出来的，其命令为：
	
	git checkout -b develop master

(b)将 develop 分支发布到 master 分支
	
	# 切换到master分支
	git checkout master
	 
	# 对develop分支进行合并
	git merge --no-ff develop

注：Git Merge 在默认情况下是执行“快进式合并”，即将 master 分支直接指向 develop 分支，并没有建立新的节点。为了保证版本演进的清晰，通常采用正常合并，通过使用参数 --on-ff，master 分支上生成一个新节点。

2.功能分支(feature-*)
(a)Git 创建功能分支
feature 分支是从 develop 分支中分出来的，其命令为：
	
	# x 版本号，如：1.1
	git checkout -b feature-x develop

(b)将 feature 分支合并到 develop 分支 
开发完成后，需要将功能分支合并到开发分支，其命令为：

	git checkout develop

	# x 版本号，如：1.1
	git merge --no-ff feature-x 

(c)删除功能分支

	# x 版本号，如：1.1
	git branch -d feature-x

3.预发布分支(release-*)
(a)Git 创建预发布分支
预发布分支是从 develop 分支中分出来的，其命令为：
	
	# x 版本号，如：1.1
	git checkout -b release-x develop

(b)将预发布分支合并到 master 分支 
预发布分支测试没有问题后，需要合并到 master 分支，其命令为：

	git checkout master

	# x 版本号，如：1.1
	git merge --no-ff release-x 

	# 对合并生成的新节点，做一个标签(打上版本标签)【x 版本号，如：1.1】
	git tag -a x

(c)删除功能分支

	# x 版本号，如：1.1
	git branch -d release-x

4.修复 Bug 分支(fixbug-*)
(a)Git 创建 Bug 分支
修复 Bug 分支是从 master 分支中分出来的，其命令为：
	
	# x 版本号，如：1.1.1
	git checkout -b fixbug-x master

(b)将修复 Bug 分支合并到 master 分支 
bug 修复完成后，需要合并到 master 分支，其命令为：

	git checkout master

	# x 版本号，如：1.1.1
	git merge --no-ff fixbug-x 

	# 对合并生成的新节点，做一个标签(打上版本标签)【x 版本号，如：1.1】
	git tag -a x

(c)将修复 Bug 分支合并到 develop 分支
	
	git checkout develop

	# x 版本号，如：1.1.1
	git merge --on-ff fixbug-x

(d)删除修复 Bug 分支

	# x 版本号，如：1.1
	git branch -d release-x


总结
在 Git 系统中合并代码有 git merge 和 git rebase 两种方式，而 git rebase 不常用。

rebase 的优势在于项目的历史提交信息非常完整。
rebase 的劣势在于安全性和可跟踪性。
rebase 的黄金法则：“绝对不要在公共分支上使用它”。