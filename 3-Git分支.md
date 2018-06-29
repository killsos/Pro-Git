## 3-Git分支

#### 3.1 分支机制简述

- Git
	
	采用一系列快照的方式
	
	Git存储的是提交对象 commit object 其中包含了指向暂存区快照的指针
	
- Git分支
	
	是一个指向某次提交的轻量级的可移动指针 movable pointer
	
	Git默认的分支名称是master
	
	分支实际上就是一个简单文件 包含了该分支所指向提交的长度为40个字符的SHA-1校验和

- Git当前分支
	
	实际上Git维护着一个名为HEAD的特殊指针
	
	在Git中 HEAD是一个指向当前所在的本地分支的指针

##### 3.1.1 创建新分支
	
	$ git branch branchName
	
	$ git log --oneline --decorate
	查看各个分支当前所指向的对象
	
##### 3.1.2 切换分支
	
	$ git checkout branchName
	
	分支切换会更改工作目录文件
	
	当你在Git中切换分支时候 工作目录的文件会被改变
	
	如果切换到较旧的分支 工作目录会被恢复到该分支上最后一次提交的状态
	
	如果在当前状态下无法干净地完成恢复操作 就不能切换分支
	
	
	$ git log --oneline --decorate --graph --all
	

#### 3.2 基本的分支和合并操作

##### 3.2.1 基本的分支操作

	$ git checkout -b branchName
	在当前所在的分支上创建一个新分支
	
	注意点:
		
		如果工作目录或者暂存区存在着未提交的更改
		
		并且这些更改与要切换到的分支冲突 Git不允许切换分支
		
		在切换分支时候 最好是保持一个干净的工作区域
			
			保持干净工作区域的方法:
			
				* 储藏提交
				
				* 修订提交
				
##### 3.2.2 基本的合并操作
	
	$ git merge 


##### 3.2.3 基本的合冲突处理

- 合并提交
	
	就可能存在冲突的可能性
	
- 处理冲突



#### 3.3 分支管理

- 查看分支

	$ git branch
	
- 查看分支最新提交

	$ git branch -v

- 查看当前分支 已合并的分支
	
	$ git branch --merged
	
- 查看当前分支 未合并的分支

	$ git branch --no-merged
	
- 删除分支

	$ git branch -d branchName
	
#### 3.4 与分支有关的工作流

- 长期分支

- 主题分支

#### 3.5 远程分支

- 远程分支
	
	是指向远程仓库的分支的指针
	
	这些指针存在于本地且无法移动
	
	
- 远程分支表现形式
	
	remote/branch
	
##### 3.5.1 推送
	
	$ git push remote localBranch:remoteBranch
	
	上面如果本地分支与远程分支名相同 可以简写为:
	
	$  git push remote branch
	

- 设置 凭据缓存
	
	不想每次推送输入密码 设置凭据缓存后 凭据信息保存在内存中几分钟
	
	$ git config --global credential.helper cache
	
- 获取远程数据

	$ git fetch origin
	
		获取服务器上的数据 如果获取到了本地还没有的新的远程跟踪分支
	
		这时Git并不会自动提供给新的远程分支的本地可编辑副本 也就是 不会在本地自动创建新的远程分支的本地分支
	
	假如 在远程服务器上有一个新分支 Branch-A
	
		这时候先拉取远程服务器数据: $ git fetch origin 
	
		基于远程Branch-A的创建本地分支: $ git checkout -b Branch-A origin/Branch-A
		
	基于本地当前分支合并远程分支
		
		$ git fetch origin 
	
		$ git merge origin/Branch-A 
			 

##### 3.5.2 跟踪分支

- 跟踪分支 tracking branch

	基于远程分支创建的本地分支自动成为跟踪分支
	
	或者叫 上游分支 upstream branch

	跟踪分支是远程分支直接关联的本地分支 如果你正处在一个跟踪分支上 并键入 git push
	
	Git会知道要将数据推送到服务器的相应的分支
	
	同样地 执行 git pull 时候 Git也能够知道从服务器拉取数据 并与本地分支进行合并
	
- 克隆分支
	
	Git默认情况下回自动创建跟踪远程origin/master分支的本地master分支
	
	$ git checkout -b [branch] [remote-branch]
	
	$ git checkout --track [remote-branch]
	
- 创建分支同时设置跟踪分支
	
	如果执行创建分支的时候 分支名与远程分支名一致的时候
	
	会自动创建跟踪分支
	
	$ git  checkout branch
	
- 设置不同名的分支的远程分支 
	
	$ git checkout -b localBranch origin/serverfix
	
- 更改跟踪分支

	$ git branch -u origin/serverfix
	
	$ git branch --set-upstream origin/serverfix
	
	设置当前分支的远程跟踪分支origin/serverfix
	
- 上游分支的简单写法

	上游分支简写 @{u}  @{upstream}
	
- 查看跟踪分支
	
	$ git branch -vv
	

##### 3.5.3 拉取

	$ git fetch
	只拉取远程的数据 不会合并
	
	$ git pull
	拉取远程的数据 并且合并的数据
	
##### 3.5.4 删除远程分支

	$ git push origin --delete branchName
	
#### 3.6 变基
	
	在Git合并分支有两种操作:
		
		* 合并 merge
		
		* 变基 rebase
		
- 变基
	
	在Git中 使用 rebase命令
	
	会把某个分支上所有提交的更改在另一个分支上重现一遍
	
	$ git rebase branceA
	
	当前分支与branceA进行变基
	
- 变基原理

	首先找到两个要整合的分支(当前所在的分支 和 要整合到的分支 )的共同祖先
	
	然后取得当前所在分支的每次提交引入的更改(diff) 并把这些更改保存为临时文件
	
	这之后将当前分支重置为要整合到的分支 最后在该分支上依次引入之前保存的每个更改
	
	优点:
		
		变基方式可以获得更简洁的提交历史
	
	注意点:
		
		不管是变基操作后最新的提交 还是 合并操作后最终的合并提交
		
		这两个提交的快照内容是完全一样的
		
		这两种操作的结果区别只是得到的提交历史不一样
		
		总结:
			
			变基操作是把某条开发分支线上的工作在另一个分支线上按顺序重现
			
			合并操作是找出两个分支的某端 并把他们合并到一起
			
		
##### 3.6.1 变基操作的危害

- 缺点
	
	不要对已经存在于本地仓库之外的提交执行变基操作
	
	也就是 既不要对已经推送到远程服务器的公开提交进行变基操作
	
		公开提交:
		
			就是已经有跟踪分支的本地分支
	
	原因:
		
		执行变基操作时候 实际上是抛弃了已有的某些提交 随后创建了新的对应提交
		
		新提交和原有的提交虽然内容上相似 但是实际上它们是不同的提交
	