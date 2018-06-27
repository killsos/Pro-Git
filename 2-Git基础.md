## 2-Git基础

#### 2.1 获取Git仓库

##### 2.1.1 现有目录中初始化Git仓库

- **初始化**
	
	$ git  init
	
	创建一个名为.git目录
	
	git add *.c // 添加所有c文件
	
	git commit -m "message"
	

##### 2.1.2 克隆现有仓库

- **克隆**
	
	$ git clone [url]
	
	$ git clone [url] [克隆在目录]
	

	
#### 2.2 在Git仓库中记录变更

工作目录下每一个文件都处于两种状态之一:
	
	已跟踪	tracked
	
	未跟踪	untracked
	
已跟踪文件
	
	是指上一次快照中包含的文件
	
	这些文件可以分为三种状态:
		
		未修改
		
		已修改
		
		已暂存
		
未跟踪文件
	
	是工作目录中出去已跟踪文件之外的所有文件
	
	也就是既不在上一次快照中 也不在暂存区中的文件
	
##### 2.2.1 查看当前文件状态
	
	$ git status
	

##### 2.2.2 跟踪文件

	$ git add fileName/directoryName
	
	开始跟踪文件或目录
	
##### 2.2.3 暂存已修改文件
	
	$ git add 
	
##### 2.2.4 显示更简洁的状态信息

	$ git status -s
	
	??  未跟踪文件
	
	A   已暂存的新文件
	
	M   已修改的文件
	
##### 2.2.5 忽略文件
	
	不希望某一类文件被Git自动添加 不想这些文件被显示在未跟踪的文件列表
	
	创建一个.gitignore文件
	
	$ cat .gitignore
	
- gitignore文件的匹配模式
	
	* 空行或者以#开始的行会被忽略
	
	* 支持标准的glob模式
	
	* 以斜杆 / 开头的模式可用于禁止递归匹配
	
	* 以斜杆 / 结尾的模式表示目录
	
	* 以感叹号 ！开始的模式表示取反
	
- glob模式

	类似于shell所使用的简化版正则表达式
	
	* 星号  零个或更多字符
	
	* 中括号 [] 表示匹配中括号内的任意单个字符
	
	* 问号 ？匹配任意单个字符
	
	* 方括号中使用中横线 - 分隔两个字符 [0-9] 匹配这两个字符范围内任何单个字符
	
	* 用两个星号 ** 匹配嵌套目录
	
	*.a                  # 忽略.a类型文件
	
	!lib.a  			 # 依然跟踪lib.a
	
	/TODO                # 只忽略当前目录的TODO文件 而不忽略子目录下的TODO
	
	build/               # 忽略build目录下的所有文件
	
	doc/*.txt            # 忽略doc直接子目录的所有.txt文件  而非直接子类的所有.txt文件不被忽略
	
	doc/**/*.pdf         # 忽略doc/目录下的所有.pdf文件
	
##### 2.2.6 查看已暂存和未暂存的变更

- git status

- git diff 
	
	输出是补丁 patch
	
	显示了工作区与暂存区的差别

- git diff --staged/--cached
	
	查看暂存区 与 历史区 的区别
	
##### 2.2.7 提交变更

- git commit 
	
	会打开默认编辑器
	
- git commit -m "message"

##### 2.2.8 跳过暂存区

	$ git commit -a -m "message"
	
	注意 修改文件必须提交过一次 新文件这个命令不可以
	
##### 2.2.9 移除文件
	
- git rm fileName
	
	* 可以删除暂存区的文件
	
	* 可以删除工作区的文件 删除了工作区的文件同时也删除了暂存区的文件
	
- 如果没有使用git rm  删除工作区的文件
	
	同时希望将工作区的删除状态同步到暂存区

	$ git rm fileName

- 更改了文件 并已经加入到暂存区 删除该文件

	$ git rm -f fileName

	加 -f 为了防止没有被记录到快照中的数据被意外移除而设立的安全特性

- 保留工作区的文件 只删除暂存区的文件

	$ git rm --cached fileName

##### 2.2.10 移动文件/重命名文件

	$ git mv file_from file_to

	===

	$ mv file_from file_to

	$ git rm file_from

	$ git add file_to


#### 2.3 查看提交历史


