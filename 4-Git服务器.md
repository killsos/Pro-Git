## 4-Git服务器

#### 4.1 协议

Git可以使用4种主要的协议来传输数据:

	- 本地协议
	
	- HTTP协议
	
	- SSH Secure Shell
	
	- Git协议

##### 4.1.1 本地协议

	本地协议 file://
	
##### 4.1.2 HTTP协议


##### 4.1.3 SSH协议

	$ git clone ssh://user@server/project.git
	
	或者可以使用类似SCP协议简短语法
	
	$ git clone user@server:project.git
	

##### 4.1.4 Git协议

	Git自带一种特殊的守护进程 专门监听 9418 端口
	

#### 4.2 在服务器上搭建Git




#### 4.3 生成个人的SSH公钥

	默认情况下 用户的SSH密钥保存在其 ~/.ssh 目录下面
	
	公钥 id_rsa.pub
	
	私钥 id_rsa
	
- 生成密钥
	
	$ ssh-keygen
	
- 查看公钥

	$ cat ~/.ssh/id_rsa.pub


#### 4.4 设置服务器

#### 4.5 Git守护进程

- 守护进程
	
	守护进程 daemon 指
	
	后台运行的 
	
	等待特定事件发生 并
	
	提供服务的程序
	
	注意 将该服务运行于防火墙保护范围之外
	
- 架设Git协议服务

	$ git daemon --reuseaddr --bash-path=/srv/git /srv/git
	
	--reuseaddr  允许服务器无需等待旧的连接超时而直接重启
	
	--bash-path  不必使用完整路径
	
	
	