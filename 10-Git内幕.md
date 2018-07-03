## 10-Git内幕

#### 10.1 底层命令和高层命令

##### 1.1.1 本地版本控制

- 底层命令 plumbing

- 高层命令 porcelain

- 复制与克隆仓库

	copy .git 拷贝.git文件夹
	
- 忽略模式 .gitignore


- hooks目录中包含了客户端和服务器端的钩子脚本

- HEAD文件

- index文件

- objects目录

	存储了个人数据库的所有内容

-refs目录

	存储是指针
	
	指针指向数据中的提交对象
	

#### 10.2 Git对象
	
	Git是一个可以按照内容寻址的文件系统
	
	Git核心就是一个简单的 key-value 数据存储
	
	底层命令hash-object 可以将数据保存到.git目录中 返回对应的键值





#### 10.3 Git引用




#### 10.4 包文件
	


#### 10.5 引用规格

	git push origin :topic  
	
	// 删除远程topic分支
	

#### 10.6 传输协议

	哑协议 dumb
	
	智能协议 smart
	
#### 10.7 维护与数据恢复

##### 10.7.1 维护
	
	git gc --auto
	
	gc garbage collect
	
	