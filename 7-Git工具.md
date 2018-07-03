## 7-Git工具

#### 7.1 选择修订版本
	
	Git允许使用多种方法来指定某次或一定范围内的提交

##### 7.1.1 单个修订版本

	可以通过给出的SHA-1散列值来引用某次提交
	
- 短格式的SHA-1
	
	不少于4个字符
	
	通过 git log 可以查看所有的提交
	
	git log --abbrev-commit --pretty=online
	
	git log --graph
	
	通过 git show ID  可以查看该提交的具体信息
	

- 分支引用
	
	git re-parse branch
	

- reflog简称
	
	Git在后台保存一份reflog 这是一份最近几个月你的HEAD和分支引用的日志
	
	$ git reflog
	
	$ git show HEAD@{number}
	
	$ git show master@{yesterday}
	
	$ git log -g master
	
- 祖先引用
	
	指定某次引用的还可以通过它的祖先
	
	如果你在引用尾部加上一个 ^  Git会将其解释为此次提交的父提交
	
		$ git show HEAD^
	
		$ git show HEAD^2  ---第2个父提交
		
		因为只有合并提交才有多个父提交
		
		首个父提交是当你进行合并时所在的分支
		
		第2个父提交是你所合并的提交
	
	另一个指定祖先的方式是: ~  它也指向首个父提交
	
	因此 HEAD^ === HEAD~
	
		$ git show HEAD~2  ---首个父提交的首个父提交
		
	
	注意 ^2 与 ~2的区别
		
		
##### 7.1.2 提交范围

- 双点号 差集
	
	双点号 可以让Git找出那些不在同一个分支上的提交
	
	$ git log master ..experiment
	
	所有可以从experiment分支中获得而不能从master分支找那个获得的提交
	
	..表示差集运算
	
- 多点 差集
	
	$ git log refA  ..refB
	
	$ git log ^refA  refB
	
	$ git log refB --not refA 
	
    这三个结果一样 都是只属于refB 不属于refA
	
	$ git log refA  refB ^refC
	
	$ git log refA  refB --not refC
	
	所有包含在refA 或 refB中 但不包含refC
	
- 三点号   (A ∪ B) - (A ∩ B)
	
	git log master ...experiment
	
	git log --left-right master ...experiment
	
	
#### 7.2 交互式暂存

- 进入交互式暂存

	$ git add -i/-interactive
	
	update 
		
		暂存文件
	
	revert
	
		取消暂存文件
	
	diff
		
		对比暂存文件与工作区文件的差别
		
		等同于 git diff --cached
	
	patch
		
		git add -p/-patch
		
	
#### 7.3 储藏与清理
	
	储藏应用场景
		
		当你在一个某分支正在工作 突然想转到其他分支上忙些别事情
		
		这时候因为要切换分支 而切换分支的前提是: 暂存区 和 工作区是干净的
		
		这是需要将 暂存区 和 工作区的变更 储藏起来
		
		当以后切换回来 还可以还原变更
		
		解决命令 git statsh
		
   储藏 stashing
   
   		能够获得工作目录的中间状态 
		
		也就是修改过被跟踪的文件 以及 暂存的变更
		
		并将该中间状态保存在一个包含未完成变更的栈中
		
		随后可以再次恢复这些状态
		
	git statsh 
	
	git stash save
	
	git stash list
	
	git statsh apply statsh@{n}
	
	git statsh apply 默认弹出第一个
	
	git statsh drop statsh@{n}  删除某个储藏
	
	git statsh pop 应用储藏并抛弃
	
- 运用储藏
	
	git stash --keep-live
		
		该选项告诉Git不要储藏已经用git add 命令暂存过的内容
		
	git stash的另一个功能是储藏已跟踪文件之外的未跟踪文件
	
	git stash默认只保存已经索引过文件
	
	如果指定了 --index-untracked 或 -u Git会储藏所有创建过的未跟踪的文件
	
	如果指定了 --patch 选项 那么只要修改过的内容 Git一概不会储藏
	
- 从储藏中创建分支
	
	git stash branch
	
	会创建一个新分支 检出储藏工作成果时所在的提交
	
	重新应用成果 如果应用成功 就会丢掉储藏
	
- 清理工作目录
	
	git clean
		
		会从工作目录中删除所有未跟踪过的文件  但是.gitignore中例外
		 
	git stash --all
		
		删除全部内容 同时将其以储藏的形式保存
		
	git clean -d directory
	
	git clean -f force
	
	git clean -n 
	
		-n 是演习一次 看看删除那些文件
	
	git clean -x
		
		-x 删除一切文件
		
	git clean -f -i 
		
		以交互方式


#### 7.4 签署工作

	Git提供利用GPG来签署和验证工作方法

##### 7.4 .1 GPG简介

	gpg --list-keys
	列出密钥
	
	gpg --gen-key
	生成密钥
	
	git config --global user.signingkey *******
	配置git私钥
	
##### 7.4 .2 签署标签
	
	git tag -s v.1.5 -m 'my signed 1.5 tag'
	签署标签
	
	git show v.15
	查看
	
	git tag -v [tag-name]
	验证标签
	
	git commit -a -S -m 'Signed commit'
	签署提交
	
	git log --show-signature -1
	查看和验证签名
	
	git log --pretty="format:%h %G? %aN %s"
	按照格式列出签名
	
	在Git1.8.3之上版本 在合并提交时候 git merge git pull 命令可以使用
	
	--verify-signatures选项 来检查并拒绝没有携带可信GPG签名的提交
	
	git merge --verify-signatures -S [branch-name]
	对待合并的所有提交是否签署过进行了验证
	并且签署了生成的合并提交
	
#### 7.5 搜索
	
	Git grep
	可以在任何提交树或工作目录中方便的查找某个字符串或正则表达式
	
	默认情况下 grep只查找工作目录下的文件
	
	-n 输出行号
	
	-count 输出总结信息  匹配到了那些文件  每个匹配文件中有多少处匹配
	
	如果想看看所查找到的匹配属于哪个方法或函数 使用参数 -p
	
- 日志搜索
	
	git log -S searchContent --oneline
	
	-S 让Git只显示出添加过 或 删除过该字符串的那些提交
	
- 行日志搜索
	
		git log -L <start>,<end>:<file>
		
		git log -L :<funcname>:<file>
	
	可以展示代码库中某个函数或代码行的历史

#### 7.6 重写历史

- 修改最近一次提交

	对最近一次提交两件事情:
		
		修改提交信息
		
		修改由于文件添加 改动 删除所记录下的快照
	
	git commit -amend

- 修改多个提交信息
	
	
- 重排提交

- 压缩提交

- filter-branch
	

#### 7.7 重置揭秘
	
	将Git视为三棵树的内容管理器
	所谓的树
		实际上指的是 “文件的集合” 并非特定的数据结构
	
	作为一个系统 Git借助一般操作来管理及操作这三棵树
	
	树                              用途
	
	HEAD						   最近提交的快照 下次提交的父提交
	
	索引                            预计的下一次提交的快照
	
	工作目录                         沙盒


##### 7.7.1 三棵树

- HEAD
	
	HEAD是指向当前分支引用的指针 指向该分支上的最后一次提交
	
	git cat-file -p HEAD
	
	git ls-tree -r HEAD
	
- 索引 Index 暂存区
	
	索引是下一次提交 也称为 暂存区
	
	当执行 git commit 时候 要查看的区域
	
	Git会将上次检出到工作目录中的所有文件的列表填入索引
	
	他们在索引中的内容和最初被检出时候一样 随后可以将其中的某些文件替换成新的版本 
	
	git commit会将它们转换成树 用于新的提交
	
- 工作目录
	
	就是个人的工作目录 其中两棵树将各自的内容以一种高效但并不直观的方式保存在.git目录中
	
	工作目录则将其提取成实际的文件 以便于编辑
	
	可以把工作目录当做沙盒 在将内容提交到暂存区并写入历史记录之前 可以随意修改
	
##### 7.7.2 工作流

	Git的主要目的是
	
		通过操作这三棵树 以逐步好转的状态来记录项目的快照
		
		
		工作目录                              索引                                  HEAD
		
					     暂存文件
		------------------------------------->
																提交
				
											 ------------------------------------->

										 检出项目

		<----------------------------------------------------------------------------
		
##### 7.7.3 重置作用

	reset 会以特定的次序重写这三棵树 并在指定以下操作时停止
		
		1. 移动HEAD分支的指向            如果指定了  --soft 则在此停止
		
		2. 使索引看起来HEAD              除非指定了  --hard 则在此停止
		
		3. 使工作目录看起来像索引
		
	git reset HEAD~
	
	--soft  仅仅移动HEAD指向的分支 
	
	--mixed 更新索引
		
		--mixed 移动HEAD指向的分支 使用HEAD当前所指向的快照的内容来更新索引 
		
	--hard 更新工作目录
		

##### 7.7.4 利用路径进行重置

	将剩余的的操作范围限制在特定的一个或一组文件
	
	git reset file.txt
	
	等同于 git reset --mixed HEAD file.txt
	
	实际上是将 file.txt 从 HEAD 复制到 索引index 中
	
	git reset [commit-ID] [files...]
	
	
##### 7.7.5 压缩
	
	压缩提交  squashing commit
	
	
##### 7.7.6 检出 checkout

	checkout 也能操作三棵树

- 不使用路径
	
	git checkout [branch]
	
	会更新所有的三棵树 使其看起来像[branch]所指定的分支
	
	git checkout [branch] 与 git reset --hard [branch] 的区别:
		
		1. checkout不会影响工作目录 会检查并确保不会破坏已更改的文件 checkout会在工作目录中进行琐碎合并 这样所有未修改过的文件都会被更新
		
		2. reset --hard 不会检查 而只是简单进行全面替换
		
		3. reset移动的是HEAD指向的分支  checkout移动是HAED 使其指向其他分支
		
	reset移动的是HEAD指向的目标
	
	checkout移动的是HEAD本身
	
	
- 使用路径

	git checkout [branch] file
	
	不会移动HEAD指针
	
	会用指定分支的文件更新当前的暂存区文件和工作目录的文件


#### 7.8 合并高级

##### 7.8.1 合并冲突

- 中止合并
	
	git merge --abort
	尝试恢复到进行合并之前状态
	
		对于工作目录中还有: 未暂存的变更 和 未提交的变更 不能很好的处理
	
	查看执行结果 git status -sb
	

- 忽略空白符
	
	-Xignore-all-space
		完全忽略空白字符
	
	-Xignore-space-change
		将单个或多个空白字符序列视为等同
		
- 手动进行文件再合并


- 检出冲突

	git checkout --conflict=diff3 [file]
	
- 合并日志

	git log --oneline --left-right --merge
	
- 组合式差异格式
	


##### 7.8.2 撤销合并

- 修复引用

	git reset --hard HEAD~


- 还原提交

	Git可以生产一个新的提交 撤销已有提交的所有变更  这个操作称为 还原 revert
	
	git revert -m 1 HEAD
		 
		 -m 1 指明那个父节点是应该保留的主线
		 

##### 7.8.3 其他类型合并

- ours或theirs偏好
	
	git merge -Xours [branch]
	
	git merge -Xtheirs [branch]
	

- 子树合并

	
	
#### 7.9 rerere

	重用记录过的解决方案   reuse recorded resolution
	
	让Git记住一个块冲突的解决方案 如果下次再碰到相同类型的冲突 Git就可以自动解决
	
- 启动rerere
	
	git config --global rerere.enabled true
	

#### 7.10 使用Git调试

- 文件标注

	git blame -L [startLine,endLine] [file]
	
	每行 谁修改的 修改的时间
	
	git blame -C [startLine,endLine] [file]
	
	
- 二分查找
	
	git biscet 
		
		会对提交历史记录进行二分查找 帮助尽快确定问题是由那一次提交引发的
		

#### 7.11 子模块


#### 7.12 打包

	git bundle 
	
#### 7.13 替换

